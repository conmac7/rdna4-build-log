Both summaries processed in full

I have read both uploaded files, including the complete 1,692-line summary from 3:12 PM and the complete 839-line summary from 6:54 PM on November 28, 2026.

I have consolidated them chronologically, preserving earlier decisions that remain relevant and giving priority to the newer Gemma-specific decisions.

# **1. Consolidated project understanding**

## **Overall objective**

You are building a local AI infrastructure on Windows 11 using AMD ROCm/HIP, custom llama.cpp builds, GGUF models, OpenCode, OpenClaw and local OpenAI-compatible APIs.

The infrastructure has three original responsibilities:

- **Task 1:** Technical documentation, specifications and project documentation.
- **Task 2:** Actual software development and implementation.
- **Everyday AI:** General questions, research, explanations and other daily tasks.

Your priorities remain accuracy, output quality, numerical precision and reliability before inference speed.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026__03_12_31_PM.md



## **Current model inventory**

Combining both summaries gives us five selected language models.

| Model                   | Responsibility       | Current status                                     |
| ----------------------- | -------------------- | -------------------------------------------------- |
| DaisyRose-27B Q3_K_XL   | Task 1 documentation | Installed and successfully validated               |
| Swift 1.5 / Qwen3.8-27B | Task 2 coding        | Configured and previously benchmarked successfully |
| Ternary Bonsai 2.5 27B  | Secondary model      | Available, not currently a priority                |
| Gemma 4 12B Q4_K_M      | Everyday AI #1       | Downloaded; installation pending                   |
| GPT-OSS 20B Q8_0        | Everyday AI #2       | Selected; installation pending                     |

The vision projectors and MTP draft models are additional supporting model files, not additional independent everyday language models.

The original division between DaisyRose and Swift remains intact.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026__03_12_31_PM.md

SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



## **Hardware and infrastructure**

**Main hardware**

CPU

**AMD Ryzen 7 5700X**

GPU

**RX 9070 XT 16GB**

System RAM

**32GB DDR4-3200**

Architecture

**AMD RDNA4 / gfx1201**

Motherboard

**Gigabyte B550 AORUS Elite V2**

OS

**Windows 11**

Main workspace: `C:\AI`

Important existing directories:

```
C:\AI\
├── DaisyRose-27B\
├── rdna4-27b-inference\
├── Bonsai demo\
├── rocm72-runtime\
└── [planned] Gemma4-12B\
```

There is an important operational restriction: DaisyRose and Swift cannot run simultaneously with their existing configurations. Continue operating with one active model at a time.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026__03_12_31_PM.md



# **2. Established configurations and completed work**

## **DaisyRose — Task 1**

Validated

The successful configuration is:

| Parameter        | Value                 |
| ---------------- | --------------------- |
| Model            | DaisyRose-27B-Q3_K_XL |
| Runtime context  | 98,304                |
| Training context | 262,144               |
| K cache          | F16                   |
| V cache          | F16                   |
| KV GPU offload   | Disabled              |
| KV streaming     | Disabled              |
| GPU layers       | 999                   |
| Parallel slots   | 1                     |
| Reasoning        | xhigh                 |
| Sampling         | 1.0 / 0.95 / 20 / 0.0 |
| Server           | `127.0.0.1:8080`      |

The most important completed experiment was the direct API test involving approximately 69,678 input tokens in one request.

Results:

- Processing completed successfully.
- All five verification answers were correct.
- Prompt processing: approximately 324 tokens/sec.
- Generation: approximately 2.18 tokens/sec.
- Total time: 6 minutes 43 seconds.
- No OpenCode compaction occurred.

**The 96K DaisyRose configuration has passed long-context validation.**

Do not repeat that experiment unnecessarily or change its established F16/F16 configuration without a specific reason.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026__03_12_31_PM.md



Additional findings:

- OpenCode's earlier multi-chunk document experiment triggered automatic compaction.
- The DaisyRose provider now has a 30-minute header timeout.
- The llama.cpp browser Web UI is disabled, but its HTTP API works.
- `Stop-DaisyRose.ps1` was generated, but installation and testing remain unconfirmed.

## **Swift 1.5 — Task 2**

Working baseline

Model:

`Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Runtime:

`C:\AI\rdna4-27b-inference`

OpenCode endpoint:

`http://127.0.0.1:18080/v1`

Your established startup sequence must remain unchanged:

```
Set-Location C:\AI\rdna4-27b-inference

$modelPath = (Resolve-Path ".\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf").Path

.\server\launch.ps1 -Model $modelPath
```

The existing Swift launcher uses `--kv-stream` and CPU placement for token embeddings.

**Never reuse or modify this launcher for Gemma.** The new Gemma deployment requires its own separate scripts.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



## **OpenCode**

Already installed and configured.

Active configuration:

`C:\Users\pgali\.config\opencode\opencode.json`

Default model:

`swift/swift15`

Existing providers include Swift, DaisyRose and Bonsai.

Important: the complete active JSON has not been reverified after every previous modification. A previously generated replacement JSON is outdated and must not overwrite your active configuration.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026__03_12_31_PM.md



## **OpenClaw**

Not installed, downloaded or configured.

Its intended purpose remains Task 1 documentation writing with DaisyRose.

# **3. New decisions and important reconciliations**

The newer summary introduces an important change in the project's immediate priorities.

**The two everyday models must now be installed before proceeding with the later OpenCode and OpenClaw integration roadmap.**

The revised order is:

1. Install Gemma 4 12B and GPT-OSS 20B.
2. Configure additional OpenCode MCP servers, addons, plugins and skills.
3. Install and configure OpenClaw for DaisyRose.
4. Configure documentation-oriented integrations.
5. Configure everyday AI integrations.&#x20;

   SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



Several apparent contradictions are resolved as follows:

| Subject                    | Consolidated interpretation                                                                                                |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| System RAM versus GPU VRAM | DaisyRose retains its validated host-side KV configuration. Gemma and GPT-OSS have the newer full-GPU-residency objective. |
| KV streaming               | Remains part of Swift's existing configuration. Explicitly excluded from Gemma.                                            |
| Context size               | DaisyRose's 96K is validated. Gemma's 8K is only a temporary smoke-test setting.                                           |
| Model count                | The original three models remain relevant; Gemma and GPT-OSS are two additional selected models.                           |
| Previous next step         | The older recommendation for DaisyRose practical evaluation is postponed by the newer everyday-model installation roadmap. |
| OpenCode/OpenClaw          | OpenCode already exists; OpenClaw remains a future installation.                                                           |

The full-VRAM requirement is especially important: Gemma's main weights, vision projector, MTP draft weights and KV cache should reside in dedicated VRAM wherever supported. We must verify actual allocations rather than trusting command-line requests.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



# **4. Gemma 4 — active installation project**

Current active task

Selected repository:

[HauhauCS / Gemma4-12B model repository](https://huggingface.co/HauhauCS/Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced?utm_source=chatgpt.com)

Three downloaded files:

| File                                                           | Purpose                        |
| -------------------------------------------------------------- | ------------------------------ |
| `Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-Q4_K_M.gguf`      | Main language model            |
| `mmproj-Gemma4-12B-QAT-Uncensored-HauhauCS-Balanced-BF16.gguf` | Vision projector               |
| `mtp-gemma-4-12B-it.gguf`                                      | MTP speculative-decoding model |

Planned installation:

```
C:\AI\Gemma4-12B\
├── models\
│   ├── [main model]
│   ├── [vision projector]
│   └── [MTP model]
├── server\
│   ├── launch-text.ps1
│   ├── launch-vision.ps1
│   └── launch-mtp.ps1
└── logs\
```

The implementation has not started.

### **Known compatibility risks**

There are three important unresolved technical concerns:

1. The pinned custom fork's Gemma 4 model-type mapping explicitly recognizes 30, 35, 42 and 60 layers, while Gemma 4 12B specifies 48 layers.
2. An upstream issue documents a Gemma 4 12B MTP warmup crash.
3. Another issue documents an MTP plus vision-projector crash under HIP/ROCm, involving the 31B variant.

These are warnings, not conclusive proof that your particular configuration will fail.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



We must validate the main model first, then vision, then MTP, and only afterwards the combined configuration.

### **GPT-OSS 20B**

The selected quantization is final:

`gpt-oss-20b-Q8_0.gguf`

Target:

- 96K context.
- Full model residency in GPU VRAM.
- Full KV-cache residency in GPU VRAM.
- No intentional system-RAM offloading.

These are targets, not verified results. GPT-OSS follows Gemma in the roadmap.&#x20;

SESSION HANDOFF SUMMARY-‎28_11_2026\_\_‏‎06_54_05_PM.md



# **5. Correct continuation point**

The latest summary explicitly identifies **Step 1: Read-only preflight**.

Before creating folders, copying files, rebuilding anything or modifying configurations, we must establish the actual state of the custom RDNA4 runtime.

The first checks are:

1. Verify the expected `llama-server.exe` exists.
2. Establish its version and build.
3. Inspect the GPU devices it recognizes.
4. Identify the exact RX 9070 XT device name.
5. Inspect the available command-line options.
6. Locate the three downloaded Gemma files.
7. Verify their original files and hashes.
8. Check whether Swift is currently running and identify its port.

No changes to the existing Swift environment.

## **Step 1A — Verify the custom inference executable**

Read-only operation

Open PowerShell and execute this single block:

```
$exe = "C:\AI\rdna4-27b-inference\build\src\build-rocm\bin\llama-server.exe"

if (-not (Test-Path -LiteralPath $exe -PathType Leaf)) {

    Write-Host "EXECUTABLE NOT FOUND:" -ForegroundColor Red
    Write-Host $exe

}
else {

    Write-Host "`n===== EXECUTABLE =====" -ForegroundColor Cyan

    Get-Item -LiteralPath $exe |
        Select-Object FullName, Length, LastWriteTime |
        Format-List

    Write-Host "`n===== VERSION =====" -ForegroundColor Cyan

    & $exe --version 2>&1

    Write-Host "`n===== GPU DEVICES =====" -ForegroundColor Cyan

    & $exe --list-devices 2>&1

    Write-Host "`n===== RELEVANT OPTIONS =====" -ForegroundColor Cyan

    $help = & $exe --help 2>&1

    $help | Select-String -Pattern `
        "list-devices|n-gpu-layers|ngl|spec-draft|mmproj|kv-offload|op-offload|flash-attn|fit|device"
}
```

This does not modify your installation, configuration, downloaded files or existing Swift environment.

**Send me the complete PowerShell output.**

We will verify it before proceeding to the next preflight check.

The established project direction is now clear: preserve DaisyRose and Swift, install Gemma 4 independently on your existing custom RDNA4 HIP runtime, validate full GPU residency and functionality, and then proceed to GPT-OSS.

No completed work needs to be repeated, and no later roadmap phase needs to be started yet.