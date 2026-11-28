I completed the first compatibility audit. There is one important finding that changes how we should install Gemma:

We should not launch all three GGUF files together as the first test. Your custom runtime has some Gemma 4 support, but I found two specific compatibility concerns that need to be tested before we rely on MTP.

Your existing rdna4-27b-inference runtime is based on a pinned llama.cpp-derived fork, not a current, unmodified upstream llama.cpp installation. Its build targets your RX 9070 XT (gfx1201), but its documented performance testing is for Swift 1.5/Qwen3.8-27B—not Gemma 4. 

GitHub+2

Compatibility findings
Component	Source-level finding	My assessment
Gemma 4 main model	Gemma 4 architecture implementation exists, but the pinned model-type mapping does not explicitly identify the 48-layer 12B variant.	Must test
BF16 mmproj	The fork recognizes the Gemma4V projector and has a corresponding image-processing graph.	Promising
MTP draft model	An upstream issue documents a crash involving Gemma 4 12B MTP warmup on Windows/AMD.	Significant risk

Sources: 

raw.githubusercontent.com+5

The MTP issue is particularly relevant: it concerns the Gemma 4 12B family and the Unsloth MTP draft model. I also found a separate report involving Gemma 4, MTP and mmproj together under ROCm. These reports do not prove your exact files will fail, but they make an immediate all-three launch an unjustified gamble. 

GitHub+2

We can still install all three files. The plan is to make the main model and image functionality work first, then test MTP independently without risking your working Swift installation.
Gemma 4 12B — Installation and configuration plan
Target environment
Existing RDNA4 runtime

Component	Configuration
OS	Windows 11
GPU	Radeon RX 9070 XT 16GB
Backend	AMD HIP / ROCm
Inference engine	rdna4-27b-inference
Main model	Gemma 4 12B Q4_K_M
Vision	BF16 mmproj
Acceleration	MTP, subject to compatibility test
Client	OpenCode


1. Directory structure
We'll keep your existing RDNA4 project intact and create a dedicated Gemma workspace.
C:\AI\
│
├── Bonsai demo\
│
├── DaisyRose-27B\
│
├── rdna4-27b-inference\
│   ├── build\
│   ├── models\
│   │   └── Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf
│   └── server\
│       └── launch.ps1          (UNCHANGED)
│
├── rocm72-runtime\
│
└── Gemma4-12B\
    │
    ├── models\
    │   ├── Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-Q4_K_M.gguf
    │   ├── mmproj-Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-BF16.gguf
    │   └── mtp-gemma-4-12B-it.gguf
    │
    ├── server\
    │   ├── launch-text.ps1
    │   ├── launch-vision.ps1
    │   └── launch-mtp.ps1
    │
    └── logs\


We'll copy, not move, your existing downloads. This gives us a clean, independent workspace.
2. Verify the existing inference engine
No new llama.cpp installation planned
Your existing project builds a Windows HIP/ROCm llama.cpp-derived runtime for gfx1201, using the pinned achyudh/llama.cpp commit and its RDNA4 Windows patch. 

raw.githubusercontent.com+1

Its existing executable is expected at:
C:\AI\rdna4-27b-inference\build\src\build-rocm\bin\llama-server.exe


We will verify:
1. The executable exists.
2. Its version and build information.
3. --list-devices identifies your RX 9070 XT.
4. The executable supports the necessary Gemma, vision, GPU-offloading and speculative-decoding parameters.
5. The existing Swift server can continue using its current configuration.
Important: We will not rebuild or update your existing runtime unless testing establishes that it is necessary.
3. Prepare the model files
We will locate your three existing downloads under C:\AI, verify their filenames, sizes and SHA256 hashes, and copy them into:
C:\AI\Gemma4-12B\models\


We'll then verify the copied files against their originals.

No additional model downloads should be necessary.
4. Configure full GPU execution
This is an important requirement throughout the installation.

The existing Swift launcher is not suitable for Gemma as-is. It uses --kv-stream, which keeps most KV cache in host RAM, and explicitly places token embeddings on the CPU. Those settings contradict your desired deployment. 

raw.githubusercontent.com+1

We'll create separate Gemma launchers that do not use those settings.

The intended configuration is:
Component	Placement
Main model weights	RX 9070 XT VRAM
Main model layers	All GPU layers
Token embeddings	GPU, where supported
KV cache	RX 9070 XT VRAM
Vision projector	RX 9070 XT VRAM
MTP draft model	RX 9070 XT VRAM, if supported
CPU model offloading	None
KV streaming to RAM	Disabled

The pinned runtime supports the relevant controls, including --gpu-layers all, --kv-offload, --mmproj-device, --spec-draft-device and --spec-draft-ngl all. 

raw.githubusercontent.com+3

We'll also use --fit off to disable automatic adjustment of unset parameters to fit device memory. If the chosen configuration cannot fit, we want to detect that rather than silently accepting a different configuration. 

raw.githubusercontent.com

Actual placement will be verified from llama.cpp's startup logs and GPU memory measurements.
5. Install and test the main model first
First inference test
We'll initially load only:

Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-Q4_K_M.gguf

The initial test will use:
- HIP/ROCm GPU.
- All available model layers offloaded.
- GPU KV cache.
- Flash Attention enabled.
- One inference slot.
- A conservative temporary context size of 8K.
The 8K is only a compatibility smoke test, not a decision about your final context capacity.

Why start without the other two files?

Because we need to establish that the main 12B model actually loads correctly with this particular pinned runtime.

There is a specific issue worth checking: the official Gemma 4 12B configuration specifies 48 layers, whereas the pinned fork's Gemma 4 model-type mapping explicitly recognizes 30, 35, 42 and 60 layers, but not 48. This is not proof of failure; the general Gemma 4 implementation exists. However, it means we must test rather than assume compatibility. 

google/gemma-4-12B at main+2

We'll test:
- Model loading.
- Basic text generation.
- Multi-turn conversations.
- Correct chat template and reasoning behaviour.
- GPU memory placement.
6. Add image understanding
Once text generation works, we'll create the second launcher:

launch-vision.ps1

This adds:
--mmproj "mmproj-Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-BF16.gguf" `
--mmproj-offload `
--mmproj-device "<YOUR_GPU_DEVICE>"


The RDNA4 fork contains Gemma4V and Gemma4UV projector graph implementations. We'll confirm which projector architecture your file identifies during loading. 

raw.githubusercontent.com+1

We'll then test actual image input, not just successful model loading.
7. Test MTP acceleration
This is the most uncertain part.

HauhauCS documents MTP and vision configurations separately, and reports approximately 60% generation improvement from its MTP configuration. 

Hugging Face

However, upstream llama.cpp has a documented Gemma 4 12B MTP warmup crash, reproduced on Windows 11 with AMD RDNA4 Vulkan and also on CPU. The issue is marked closed as not planned. 

GitHub+1

There is also a separate report involving Gemma 4 MTP plus mmproj on HIP/ROCm 10.1, although that report concerns the 31B variant. 

GitHub+1

Therefore, we'll test in this order:
1. Main model + MTP, without mmproj.
2. Verify MTP draft-model GPU allocation.
3. Test speculative decoding and generation.
4. Only then combine MTP and mmproj.
The MTP launch parameters will include:
-md "mtp-gemma-4-12B-it.gguf" `
--spec-type draft-mtp `
--spec-draft-device "<YOUR_GPU_DEVICE>" `
--spec-draft-ngl all


These are supported parameter names in the pinned fork. 

GitHub+1

If MTP fails, we won't compromise the working main model and vision functionality. We'll investigate a suitable fix or a side-by-side updated RDNA4 build, without modifying the Swift installation.
8. Configure the local API
Once the inference server works independently, we'll expose it locally.

Proposed endpoint:
http://127.0.0.1:18081/v1


Your existing Swift server uses port 18080. We'll verify that 18081 is free before using it. 

raw.githubusercontent.com+1

We'll test:
- /health
- /v1/models
- /v1/chat/completions
- Image requests once vision is enabled.
This separates model/runtime problems from OpenCode configuration problems.
9. Connect Gemma to OpenCode
Client integration
We'll add a new local provider to your existing OpenCode configuration.

The official OpenCode documentation supports custom OpenAI-compatible servers through:

@opencode/ai/providers/openai-compatible

The provider needs the actual server URL, model ID, context limit, output limit and supported input/tool capabilities. 

OpenCode

Our procedure:
1. Back up your existing OpenCode configuration.
2. Add a separate Gemma provider.
3. Preserve your existing Swift provider and default settings.
4. Obtain the actual model ID from /v1/models.
5. Configure text and image capabilities only after testing them.
6. Test conversations directly inside OpenCode.
We won't configure MCP servers, skills or plugins yet. Those belong to later phases of your roadmap.
10. Final acceptance testing
The installation will only be considered complete when we can verify these results:
Test	Acceptance condition
Main model	Loads and generates answers
GPU model placement	All applicable weights on RX 9070 XT
KV cache	GPU allocation confirmed
Vision	Real image successfully analysed
MTP	Successful warmup and speculative generation, if compatible
OpenAI API	Working local endpoints
OpenCode	Gemma selectable and usable
Swift	Existing configuration remains functional
GPU memory	No intentional model/KV allocation in system RAM

We'll use actual startup logs, dedicated GPU memory measurements and inference tests. Merely specifying GPU flags won't count as proof.

11. Verification of this guide
I cross-checked the plan against four source groups:
Area	Verification
RDNA4 build and Windows HIP	Build script and repository documentation
Gemma 4 architecture	Pinned runtime source and Google's official configuration
GPU and MTP parameters	Pinned runtime argument parser
MTP compatibility	Upstream Gemma 4 bug reports
OpenCode integration	Current official OpenCode documentation
Model files and intended usage	HauhauCS model card

Guide audited against source
There are no intentionally unsupported launch flags in the proposed design. However, the actual installed binary's version and local environment still need preflight verification. The Gemma 12B model classification and MTP concerns also remain real compatibility tests, not resolved facts.
Implementation sequence
Implementation checklist

0/8 completed



[ ] Step 1: Read-only preflight: runtime, GPU, CLI, ports and downloaded files
[ ] Step 2: Create Gemma directory and verify/copy all three GGUF files
[ ] Step 3: Test main Gemma model with HIP and GPU-resident KV cache
[ ] Step 4: Add mmproj and test image understanding
[ ] Step 5: Test MTP acceleration independently
[ ] Step 6: Combine the working model components and finalize launch scripts
[ ] Step 7: Test API and integrate Gemma into OpenCode
[ ] Step 8: Run final GPU residency and functionality checks
This checklist is for tracking our implementation together. Nothing has been installed or changed yet.

My recommendation: We start with Step 1, the read-only preflight. We won't create directories, copy files, rebuild anything, or modify OpenCode until the relevant checks have passed.

That way, we protect your existing Swift environment and build Gemma as a separate, controlled installation.