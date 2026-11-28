# SESSION HANDOFF SUMMARY
## Local LLM Infrastructure, OpenCode, llama.cpp ROCm, DaisyRose 27B and Swift 1.5 27B

**Session date:** November 28, 2026  
**Environment:** Windows 11  
**Primary objective:** Establish a reliable, locally hosted AI development and documentation environment using OpenCode, custom llama.cpp builds and multiple local GGUF models.

---

# 1. PROJECT OVERVIEW AND OBJECTIVES

The project involves building and maintaining a local AI environment on a Windows 11 PC, with locally hosted GGUF models served through llama.cpp and accessed through OpenCode.

The user has three installed language models:

1. DaisyRose-27B-Q3_K_XL.gguf
2. Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf
3. Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf

There is also a separate Bonsai multimodal projector file.

The intended division of responsibilities is:

| Task | Model | Priority |
|---|---|---|
| Task 1 | DaisyRose 27B | Writing documentation, technical explanations and documentation quality |
| Task 2 | Swift 1.5 27B | Software development, programming, implementation and coding |
| Secondary | Ternary Bonsai 2.5 27B | Not a current priority |

The user is particularly interested in evaluating the actual quality of the models for their intended tasks.

Important preferences:

- Accuracy and output quality take priority over inference speed.
- Avoid unnecessary quantization of internal inference data.
- Prefer F16 K/V cache when feasible.
- Large context windows are desirable, especially for technical documentation and coding.
- The user is willing to use system RAM for KV cache rather than relying exclusively on GPU VRAM.
- A configuration that works reliably is preferable to an aggressive configuration that frequently runs into memory or timeout problems.
- Never change working configurations without explaining the reason and preserving a rollback path.

The objective is not simply to make the models run. It is to obtain the best possible quality and usable context capacity from the existing hardware.

---

# 2. HARDWARE AND OPERATING ENVIRONMENT

## 2.1 Main PC

| Component | Configuration |
|---|---|
| CPU | AMD Ryzen 7 5700X |
| CPU cores | 8 cores / 16 threads |
| GPU | Gigabyte Radeon RX 9070 XT GAMING OC |
| GPU architecture | RDNA4 |
| GPU memory | 16GB GDDR6 |
| System RAM | 32GB DDR4-3200 |
| Motherboard | Gigabyte B550 AORUS Elite V2 rev. 1.2 |
| Operating system | Windows 11 |
| GPU driver | AMD Adrenalin (exact currently installed version not verified in this session) |

Important hardware constraint:

The PC has only 32GB physical system RAM and 16GB GPU VRAM.

Large F16 KV caches can consume considerable system RAM when `--no-kv-offload` is used.

## 2.2 Main software

- Windows PowerShell.
- OpenCode v2.3.2.
- llama.cpp custom builds.
- AMD ROCm/HIP runtime components.
- GGUF models.
- OpenAI-compatible local HTTP APIs.

The user primarily operates the environment through PowerShell.

---

# 3. DIRECTORY STRUCTURE

The main AI directory is:

`C:\AI`

The following directories have been identified:

```text
C:\AI\
│
├── DaisyRose-27B\
│   ├── logs\
│   ├── models\
│   ├── llama.cpp\
│   └── [additional entries]
│
├── rdna4-27b-inference\
│   ├── models\
│   ├── build\
│   ├── .git\
│   ├── docs\
│   ├── include\
│   ├── lib\
│   ├── patches\
│   ├── scripts\
│   ├── prune\
│   └── server\
│
├── Bonsai-demo\
│   ├── models\
│   ├── scripts\
│   ├── tests\
│   ├── .git\
│   ├── .github\
│   ├── assets\
│   ├── community-benchmarks\
│   └── demos\
│
└── rocm72-runtime\
    ├── Scripts\
    ├── include\
    ├── Lib\
    └── pyvenv.cfg
```

The `rocm72-runtime` directory appears to be a Python virtual environment with installed packages.

The complete package names were not sufficiently legible in the original screenshots.

**Important:** DaisyRose's launcher references its own runtime directories under `C:\AI\DaisyRose-27B\rocm-runtime`. Do not automatically assume that this is the same environment as the separate `C:\AI\rocm72-runtime` directory.

---

# 4. EXACT MODEL INVENTORY

There are THREE independent language models, not four.

## 4.1 DaisyRose 27B

Exact filename:

`DaisyRose-27B-Q3_K_XL.gguf`

Location:

`C:\AI\DaisyRose-27B\models\DaisyRose-27B-Q3_K_XL.gguf`

Official sources:

- https://huggingface.co/RadixArk/DaisyRose-27B-GGUF
- https://huggingface.co/RadixArk/DaisyRose-27B-GGUF/blob/main/DaisyRose-27B-Q3_K_XL.gguf
- https://huggingface.co/RadixArk/DaisyRose-27B

Verified model metadata returned by llama.cpp:

| Property | Value |
|---|---:|
| Parameters | 27,320,697,856 |
| GGUF file size | 13,212,073,984 bytes |
| Quantization | Q3_K - Medium |
| Vocabulary | 248,320 |
| Embedding dimension | 5,120 |
| Model training context | 262,144 |
| Initial runtime context | 65,536 |
| Current tested runtime context | 98,304 |

DaisyRose is intended for Task 1: documentation and technical writing.

## 4.2 Swift 1.5 / Qwen3.8-27B

Exact filename:

`Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Location:

`C:\AI\rdna4-27b-inference\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Official source:

https://huggingface.co/Safasfly/Swift-1.5-Qwen3.8-27B-GSQ-RCO-IQ3_S-P2-MTP1-GGUF

Exact model file:

https://huggingface.co/Safasfly/Swift-1.5-Qwen3.8-27B-GSQ-RCO-IQ3_S-P2-MTP1-GGUF/blob/main/Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf

Swift is paired specifically with the custom `rdna4-27b-inference` project.

It is intended for Task 2: programming and software development.

Its OpenCode configuration declares:

- Context: 262,144 tokens.
- Output: 16,384 tokens.
- Default reasoning effort: xhigh.
- Additional reasoning variant: low in the original configuration.

A medium reasoning variant was planned as part of the subsequent configuration cleanup.

Whether the user applied every change in the generated replacement JSON needs verification.

## 4.3 Ternary Bonsai 2.5 27B

Exact filenames:

`Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf`

`Ternary-Bonsai-2.5-27B-mmproj-Q8_0.gguf`

The first is the language model.

The second is a multimodal projector, not an independent fourth language model.

The screenshot showed this directory:

`C:\AI\Bonsai-demo\models\bonsai25.gguf\27B`

However, the OpenCode configuration supplied by the user references:

`C:\AI\Bonsai-demo\models\bonsai25-gguf\27B\Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf`

There is a possible directory-name discrepancy between `bonsai25.gguf` and `bonsai25-gguf`. Verify the actual path before changing anything.

The user is not currently interested in optimizing Bonsai.

---

# 5. CUSTOM RDNA4 LLAMA.CPP BUILD

Repository:

https://github.com/lnieuwenhuis/rdna4-27b-inference

User's build log repository:

https://github.com/conmac7/rdna4-build-log

The custom build was developed for RDNA4 / RX 9070 XT.

## 5.1 Historical build issue

The build initially failed during embedded Web UI asset provisioning.

The build log showed:

- Matching `b1` UI checksum download failed.
- The fallback downloaded a newer `latest` bundle.
- The newer bundle used a different `_app/immutable/` layout.
- The pinned llama.cpp source expected legacy assets such as:
  - `bundle[hash].js`
  - `bundle[hash].css`
  - `workbox[hash].js`

This caused the asset validation failure.

The solution was to add:

`-DLLAMA_USE_PREBUILT_UI=OFF`

to the final CMake configuration arguments.

This was necessary even though `LLAMA_BUILD_UI` was already OFF, because the prebuilt UI download was controlled by a separate option.

The failed UI cache was cleaned, and the build was repeated successfully.

The embedded Web UI is intentionally disabled.

## 5.2 Important Web UI distinction

The user cannot use the browser-based llama.cpp Web UI.

Do not tell the user to visit:

`http://127.0.0.1:8080`

expecting a browser interface.

The API is still accessible programmatically.

These PowerShell operations were successfully performed:

- `http://127.0.0.1:8080/health`
- `http://127.0.0.1:8080/v1/models`
- `http://127.0.0.1:8080/tokenize`

The server is launched with `--no-ui`.

**Missing Web UI does not mean missing HTTP API.**

Use PowerShell and HTTP API calls, not browser navigation.

---

# 6. EXACT SWIFT STARTUP PROCEDURE

This is the user's actual startup sequence. Do not replace it with a generic alternative.

Open PowerShell and execute these exact commands:

```powershell
Set-Location C:\AI\rdna4-27b-inference

$modelPath = (Resolve-Path ".\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf").Path

.\server\launch.ps1 -Model $modelPath
```

This launches the custom RDNA4 Swift inference server.

OpenCode endpoint:

`http://127.0.0.1:18080/v1`

This is the exact personally used launch procedure and must be preserved.

---

# 7. DAISYROSE STARTUP PROCEDURE

DaisyRose has a separate llama.cpp installation.

The startup file created during this work is:

`C:\AI\Start-DaisyRose.ps1`

The user runs:

```powershell
Set-Location C:\AI
.\Start-DaisyRose.ps1
```

The script is based on the user's actual earlier PowerShell startup script.

It performs the following operations:

1. Configures the local ROCm runtime DLL directories in PATH.
2. Selects `DaisyRose-27B-Q3_K_XL.gguf`.
3. Checks whether port 8080 is already occupied.
4. Starts `llama-server.exe`.
5. Redirects standard output and error to the DaisyRose log files.
6. Waits for the HTTP health endpoint.
7. Queries `/v1/models` to verify the loaded model.

Relevant paths:

```powershell
$root = "C:\AI\DaisyRose-27B"
$bin = "$root\llama.cpp\bin"
$coreBin = "$root\rocm-runtime\_rocm_sdk_core\bin"
$libsBin = "$root\rocm-runtime\_rocm_sdk_libraries\bin"
$model = "$root\models\DaisyRose-27B-Q3_K_XL.gguf"

$port = 8080
```

The current model context argument was changed during the 96K experiment.

**Current setting:**

```powershell
"-c", "98304",
```

Originally it was:

```powershell
"-c", "65536",
```

## 7.1 Current server arguments

The important launcher arguments are:

```powershell
"-m", $model,
"-ngl", "999",
"-c", "98304",
"-np", "1",
"--no-kv-offload",
"--host", "127.0.0.1",
"--port", "$port",
"--no-ui",
"--jinja",
"--temp", "1.0",
"--top-p", "0.95",
"--top-k", "20",
"--min-p", "0.0"
```

The original startup script starts the process with:

- `llama-server.exe`
- Working directory: `$bin`
- Standard output: `C:\AI\DaisyRose-27B\logs\opencode-stdout.log`
- Standard error: `C:\AI\DaisyRose-27B\logs\opencode-stderr.log`

The health-check URL is:

`http://127.0.0.1:8080/health`

The API endpoint is:

`http://127.0.0.1:8080/v1`

The user successfully started DaisyRose using this script.

---

# 8. LLAMA.CPP VERSION AND KV-CACHE OPTIONS

DaisyRose's local executable reported:

```text
version: 0.7.0-dev (build 1, commit b540a4b)
built with Clang 24.0.0 for Windows AMD64
```

The user inspected the complete `llama-server.exe --help` output.

## 8.1 Current KV-cache arrangement

The current DaisyRose launcher includes:

```powershell
"--no-kv-offload",
```

It does not explicitly specify K or V cache types.

The user's actual executable documents these defaults:

```text
--cache-type-k default: f16
--cache-type-v default: f16
```

Therefore, the current cache configuration is:

| Component | Current type |
|---|---|
| K cache | F16 |
| V cache | F16 |
| KV GPU offloading | Disabled |
| KV streaming | Disabled |

The user prioritizes numerical fidelity and output quality over speed.

The user explicitly prefers retaining F16 K/V if feasible.

## 8.2 KV streaming capability

The DaisyRose executable supports:

- `--kv-stream`
- `--kv-resident-mib`
- `--kv-resident-margin-mib`
- `--kv-resident-mib-decode`
- `--kv-stream-split`

However, the user's actual `--help` output explicitly states:

**`--kv-stream` requires all of the following:**

1. Flash Attention enabled: `-fa on`.
2. K cache type: `q8_0`.
3. V cache type: `q4_0`.

Therefore, this streaming implementation is NOT compatible with retaining F16 K and F16 V.

Do not recommend combining `--kv-stream` with F16 K/V on this executable.

The RDNA4 project documentation contains an adaptive KV streaming configuration using:

```powershell
"-fa", "on",
"-ctk", "q8_0",
"-ctv", "q4_0",
"--kv-stream",
"--kv-resident-mib", "1355",
"--kv-stream-split"
```

That configuration was designed for the custom RDNA4 inference project.

It is an option if the user later prioritizes context capacity over full F16 cache precision, but it is NOT the current DaisyRose configuration.

## 8.3 Other relevant flags

The user specifically asked about:

- `-c 65536`: context size; currently changed to 98304.
- `--no-kv-offload`: disables KV cache offloading to the GPU.
- `--jinja`: enables Jinja chat-template processing.

The inspected version says Jinja is enabled by default, but the explicit flag is retained.

---

# 9. OPEN CODE CONFIGURATION

Configuration location:

`C:\Users\pgali\.config\opencode\opencode.json`

OpenCode version observed:

`opencode v2.3.2`

Other files in the configuration directory observed during the previous session:

- `opencode-bonsai.json`
- `opencode.json`
- `service.json`

The active configuration is `opencode.json`.

A previous backup was created:

`opencode.json.backup-20261127-180128`

## 9.1 Provider/model naming

The initial configuration, called `init`, used:

```json
"model": "bonsai/bonsai25"
```

The current configuration, called `current`, changed this to:

```json
"model": "swift/swift15"
```

The distinction between the two Swift identifiers is important:

- `swift` is the provider ID.
- `swift15` is the model ID inside that provider.

There is only ONE Swift model entry.

OpenCode's model identifier uses:

`provider_id/model_id`

## 9.2 Three configured models

The current configuration has these providers:

| Provider ID | Model ID | Actual model |
|---|---|---|
| `bonsai` | `bonsai25` | Ternary Bonsai 2.5 27B |
| `swift` | `swift15` | Swift 1.5 / Qwen3.8-27B |
| `daisyrose` | `daisyrose27b` | DaisyRose 27B |

Default:

`swift/swift15`

All three models remain configured.

## 9.3 Endpoints and model paths

### Bonsai

Provider ID: `bonsai`

Model ID: `bonsai25`

Endpoint:

`http://127.0.0.1:8080/v1`

Configured model path:

`C:\AI\Bonsai-demo\models\bonsai25-gguf\27B\Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf`

Context: 131072

Output: 16384

Reasoning effort: xhigh

Capabilities include tool use and text/image input.

**Warning:** Bonsai shares port 8080 with DaisyRose. Both providers cannot use their configured endpoint for different running models simultaneously.

### Swift

Provider ID: `swift`

Model ID: `swift15`

Endpoint:

`http://127.0.0.1:18080/v1`

Model path:

`C:\AI\rdna4-27b-inference\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Context: 262144

Output: 16384

Default reasoning:

`xhigh`

Originally configured with a `low` reasoning variant.

During this work, the user requested:

- Explicit sampling values in Swift's OpenCode `body`.
- Exposure of supported reasoning alternatives.

A replacement OpenCode JSON file was generated for download, with explicit sampling parameters and reasoning variants.

However, the user did not explicitly confirm that every generated configuration change was applied to the active file.

**Verify the actual current Swift configuration before relying on the planned variants.**

The intended explicit sampling body is:

```json
"body": {
  "temperature": 1.0,
  "top_p": 0.95,
  "top_k": 20,
  "min_p": 0.0,
  "chat_template_kwargs": {
    "reasoning_effort": "xhigh"
  }
}
```

The Swift model's `low` reasoning variant should be preserved. A `medium` variant was also planned.

### DaisyRose

Provider ID: `daisyrose`

Model ID: `daisyrose27b`

Endpoint:

`http://127.0.0.1:8080/v1`

Model path:

`C:\AI\DaisyRose-27B\models\DaisyRose-27B-Q3_K_XL.gguf`

Current context limit: **98304**

Output limit: 16384

Default reasoning:

`xhigh`

Its current intended sampling body:

```json
"body": {
  "temperature": 1.0,
  "top_p": 0.95,
  "top_k": 20,
  "min_p": 0.0,
  "chat_template_kwargs": {
    "reasoning_effort": "xhigh"
  }
}
```

Compatibility:

```json
"compatibility": {
  "reasoningField": "reasoning_content"
}
```

The user verified the DaisyRose context limit through PowerShell:

```text
Model: DaisyRose-27B Q3_K_XL
Context: 98304
Output: 16384
```

## 9.4 Header timeout

A timeout issue occurred while OpenCode was using DaisyRose for a long-context test.

The DaisyRose provider's settings were changed to:

```json
"settings": {
  "baseURL": "http://127.0.0.1:8080/v1",
  "headerTimeout": 1800000
}
```

This represents 30 minutes.

The user verified:

```text
DaisyRose endpoint: http://127.0.0.1:8080/v1
Header timeout: 1800000 ms
```

Then executed:

```powershell
opencode reload
```

Result:

`Configuration reloaded`

This adjustment applies to DaisyRose's OpenCode provider.

It does NOT modify the llama.cpp server or the direct API client timeout.

---

# 10. SAMPLING PARAMETERS AND REASONING

The model authors' recommendations were investigated using their Hugging Face model repositories.

The common recommended sampling settings established during that research were:

| Parameter | Value |
|---|---:|
| Temperature | 1.0 |
| Top P | 0.95 |
| Top K | 20 |
| Min P | 0.0 |
| Default reasoning | xhigh |

These values are already explicitly configured in DaisyRose's server launcher.

The Swift server launch configuration was also previously checked and reported to use the same sampling values.

DaisyRose's launch script explicitly contains:

```powershell
"--temp", "1.0",
"--top-p", "0.95",
"--top-k", "20",
"--min-p", "0.0"
```

The DaisyRose OpenCode configuration also sends these values in `body`.

Swift's original OpenCode configuration did not explicitly send all four parameters. Making its body equally explicit was a requested consistency improvement.

Important principle:

Consistency means consistent configuration structure, not forcing every model to use identical values if its author recommends different settings.

The current working assumption is that both models should use the above common sampling configuration, subject to future model-specific benchmark evidence.

---

# 11. DAISYROSE CONTEXT EXPERIMENT

This is one of the most important parts of the session.

The initial DaisyRose runtime context was 65,536 tokens.

The user wanted to investigate whether the model could use a larger context while retaining F16 K/V and `--no-kv-offload`.

## 11.1 Baseline measurements at 64K

When DaisyRose was running at 65,536 context:

| Measurement | Result |
|---|---:|
| Total RAM | 31.93 GiB |
| Free RAM | 6.22 GiB |
| Working set | 16.01 GiB |
| Private memory | 16.62 GiB |

The model metadata reported:

- Native/training context: 262144.
- Runtime context: 65536.

## 11.2 Changed to 96K

The user edited:

`C:\AI\Start-DaisyRose.ps1`

Changed:

```powershell
"-c", "65536",
```

to:

```powershell
"-c", "98304",
```

The user verified the launcher line:

```text
Start-DaisyRose.ps1:22: "-c", "98304",
```

DaisyRose was restarted successfully.

The `/v1/models` response reported:

```json
"n_ctx": 98304,
"n_ctx_train": 262144
```

This confirmed successful initialization at 98,304 runtime context.

## 11.3 Corrected 96K memory measurements

The first measurement was incorrect because another application was open.

The corrected values were:

| Measurement | 64K | 96K |
|---|---:|---:|
| Free RAM | 6.22 GiB | 6.66 GiB |
| Working set | 16.01 GiB | 18.05 GiB |
| Private memory | 16.62 GiB | 18.66 GiB |

The working set and private memory increased by approximately 2.04 GiB.

This closely matches the estimated additional 2 GiB of F16 KV cache for the extra 32K context.

The different free-RAM figures are not directly comparable because other Windows processes may have changed their memory usage.

During a later long-running request, another measurement showed:

| Measurement | Result |
|---|---:|
| Free RAM | 5.68 GiB |
| Working set | 15.47 GiB |
| Private memory | 20.87 GiB |

This was collected during/after a prolonged context operation, not a clean idle baseline.

## 11.4 Simple inference test at 96K

A short question was sent through OpenCode:

"Explain the difference between RAM and VRAM in exactly three sentences."

DaisyRose responded with exactly three coherent sentences.

Reported performance from the screenshot:

- Generation speed: 7.4 tokens/sec.
- Generation time: 1 minute 27 seconds.

This established basic inference functionality at 96K.

It did not test a long prompt.

## 11.5 Decision

96K was retained as the experimental DaisyRose context.

The OpenCode DaisyRose limit was changed from 65536 to 98304.

The user verified the updated active configuration and reloaded OpenCode.

---

# 12. LONG-CONTEXT VALIDATION DOCUMENT

A synthetic document was created for testing:

`C:\AI\daisyrose-96k-context-test.txt`

It was generated using PowerShell and tokenized through the DaisyRose llama.cpp endpoint:

`http://127.0.0.1:8080/tokenize`

Final verified document statistics:

| Property | Value |
|---|---:|
| Lines | 1606 |
| Numbered records | 1600 |
| Characters | 257,192 |
| Actual tokens | 69,467 |

The first generator version produced 121,517 tokens and exceeded the 96K target.

It was replaced with a corrected generator using 1,600 records.

Final token count:

`69467`

## 12.1 Embedded verification values

The document includes three checkpoint values:

Beginning:

`MAPLE-ORCHID-4817`

Middle:

`COPPER-DELTA-9352`

End:

`VIOLET-HARBOR-6024`

It also includes numbered records with checksums.

The checksum formula is:

```text
((record_number * 7919 + 104729) % 999983)
```

Record 01600:

`775333`

Total numbered records:

`1600`

These values are independently verifiable from the generator.

---

# 13. FIRST LONG-CONTEXT TEST THROUGH OPENCODE

The first experiment attempted to make DaisyRose read the entire 69,467-token file through OpenCode's `read` tool.

The document was divided into nine sequential ranges:

| Chunk | Offset | Lines |
|---|---:|---|
| 1 | 1 | 1–200 |
| 2 | 201 | 201–400 |
| 3 | 401 | 401–600 |
| 4 | 601 | 601–800 |
| 5 | 801 | 801–1000 |
| 6 | 1001 | 1001–1200 |
| 7 | 1201 | 1201–1400 |
| 8 | 1401 | 1401–1600 |
| 9 | 1601 | 1601–1606 |

## 13.1 First attempt: timeout

The first attempt encountered:

`Timed out waiting for response headers`

The investigation showed:

- DaisyRose health endpoint remained `ok`.
- Its process was still running.
- Memory was not exhausted.
- Logs showed ongoing generation around 3.62 tokens/sec.
- One main generation task had produced thousands of tokens.
- Several additional task IDs received cancellation warnings.

There was no evidence of a llama-server crash.

The likely issue was OpenCode's response-header timeout combined with slow inference and tool overhead.

The DaisyRose provider's header timeout was increased to 30 minutes:

`headerTimeout: 1800000`

OpenCode successfully reloaded the new configuration.

## 13.2 Second attempt: completed with automatic compaction

After increasing the timeout, the nine-range experiment completed.

However, during the transition from range 7 to range 8, OpenCode performed automatic conversation compaction.

The screenshot showed:

`37.7K in · 5.8K out`

Afterward, the context indicator displayed approximately:

`11K`

DaisyRose continued reading range 8 and range 9 and successfully answered all five verification questions.

### Verified results

| Test | Correct answer |
|---|---|
| Beginning checkpoint | MAPLE-ORCHID-4817 |
| Middle checkpoint | COPPER-DELTA-9352 |
| End checkpoint | VIOLET-HARBOR-6024 |
| Record 01600 checksum | 775333 |
| Number of records | 1600 |

All five answers were correct.

The screenshot was interpreted as showing approximately:

- Generation speed: 5.1 tokens/sec.
- Total elapsed time: 43 minutes 23 seconds.

Treat these as screenshot-derived measurements, not independently captured server timing measurements.

### What this experiment proved

It demonstrated that DaisyRose could process the document through multiple OpenCode read operations, survive an automatic compaction event and retrieve the correct checkpoints afterward.

### What it did NOT prove

Because OpenCode compacted the conversation, we cannot claim that all 69,467 original document tokens were simultaneously present in the model's raw context at one time.

The successful retrieval could have used the compacted summary alongside later file contents.

The test was useful, but it was not a clean full-context model benchmark.

---

# 14. CURRENT TIMEOUT AND COMPACTION CONFIGURATION

OpenCode's DaisyRose provider currently has:

```json
"headerTimeout": 1800000
```

This was applied to address the earlier timeout.

The long-context OpenCode test eventually completed, but automatic compaction occurred.

The exact internal context-retention behaviour of the compaction mechanism should not be confused with the model's 98,304-token native runtime allocation.

For the next experiment, avoid OpenCode's file-reading tools and conversation compaction entirely.

---

# 15. DIRECT API TEST — NOT YET PERFORMED

This is the immediate next experiment.

The user agreed to test DaisyRose directly through llama.cpp's OpenAI-compatible API.

The objective is to send the entire 69,467-token document in a SINGLE request.

This should isolate model inference from:

- OpenCode tool calls.
- OpenCode file-reading limits.
- OpenCode automatic compaction.
- Multi-request orchestration.

The intended endpoint is:

`http://127.0.0.1:8080/v1/chat/completions`

This must be accessed through PowerShell, not a browser.

The server's API is available despite the missing Web UI.

The intended request should:

1. Read the entire file `C:\AI\daisyrose-96k-context-test.txt`.
2. Include its complete contents in a single user message.
3. Include the five verification questions.
4. Use the actual model ID returned by `/v1/models`.
5. Use the intended sampling parameters:
   - temperature 1.0
   - top_p 0.95
   - top_k 20
   - min_p 0.0
6. Request xhigh reasoning using the appropriate `chat_template_kwargs` field.
7. Limit the final answer to approximately 512 tokens.
8. Use a sufficiently long PowerShell HTTP request timeout, for example 7,200 seconds.
9. Capture the complete HTTP response and any timing information.
10. Measure system memory before and after the operation.
11. Inspect server logs for prompt-processing and generation timings.

The direct test has NOT yet been implemented or executed.

Do not assume the API response schema or timings fields without verifying them against this build.

## 15.1 Proposed preflight checks

Before making the direct request, verify:

- DaisyRose is running.
- `/health` returns `ok`.
- `/v1/models` identifies the correct GGUF.
- `/slots` shows an idle slot.
- The 69,467-token document exists.
- Free system RAM is recorded.
- No OpenCode inference request is still running against DaisyRose.

The user previously clarified that the HTTP root URL must not be used in a browser because the Web UI is disabled.

PowerShell API calls work.

---

# 16. IMPORTANT CURRENT CONFIGURATION DECISIONS

These decisions have already been made and should not be casually reversed.

| Subject | Decision | Reason |
|---|---|---|
| DaisyRose task | Documentation | Dedicated Task 1 model |
| Swift task | Coding | Dedicated Task 2 model |
| Default OpenCode model | Swift | Primary coding model |
| DaisyRose context | 98,304 experimental | Successfully initialized and used |
| DaisyRose K/V cache | F16/F16 | Maximum numerical fidelity preference |
| DaisyRose KV offload | `--no-kv-offload` | Current working memory arrangement |
| DaisyRose KV streaming | Disabled | Requires Q8 K + Q4 V on this build |
| DaisyRose sampling | 1.0 / .95 / 20 / 0.0 | Model-author recommendations already configured |
| Reasoning default | xhigh | Intended quality-oriented setting |
| OpenCode header timeout | 1,800,000 ms | Addresses long-running response-header waits |
| Embedded Web UI | Disabled | Historical UI asset build problem |
| Bonsai optimization | Deferred | Not currently important |

---

# 17. UNRESOLVED QUESTIONS AND POTENTIAL PROBLEMS

## 17.1 Can DaisyRose process the entire 69,467-token document in one request?

Not yet established.

The multi-chunk OpenCode test completed, but automatic compaction prevented a clean test of raw simultaneous context retention.

The direct API test must answer this.

## 17.2 How much RAM is consumed by a genuine 69K direct prompt?

Not yet measured.

The 96K model startup measurements are available, but a full 69K prompt can increase transient memory usage.

## 17.3 Is 96K practical for sustained documentation work?

Not yet completely established.

The server initializes and short inference works. The multi-chunk test completed, but its performance and raw-context integrity need further evaluation.

## 17.4 Should the user upgrade to 64GB RAM?

Not decided.

The existing 32GB system has limited headroom with F16 K/V and `--no-kv-offload`.

More RAM might allow larger contexts, but it is too early to recommend an upgrade without completing the direct test.

## 17.5 Should adaptive KV streaming be used?

Not for the current quality-first configuration.

The executable explicitly requires:

- Flash Attention ON.
- K cache Q8_0.
- V cache Q4_0.

This conflicts with the user's preference for F16 K/V.

A future experiment may compare quality and context capacity, but it should be kept separate from the current baseline.

## 17.6 Exact current OpenCode JSON

The user modified the active `opencode.json` and confirmed:

- DaisyRose context = 98304.
- DaisyRose headerTimeout = 1800000.
- `opencode reload` succeeded.

However, the actual entire final JSON was not pasted again after all modifications.

Verify the active values before any further configuration edits, particularly Swift's explicit sampling body and reasoning variants.

## 17.7 Current server state

The last known DaisyRose process during the 96K experiments was PID 13540.

That PID and running state may have changed.

Always check the listener on port 8080 before assuming DaisyRose is running.

---

# 18. IMPORTANT POWERSHELL COMMANDS

## Check DaisyRose server process

```powershell
Get-NetTCPConnection -LocalPort 8080 -State Listen |
    ForEach-Object {
        Get-Process -Id $_.OwningProcess |
            Select-Object Id, ProcessName, Path
    }
```

## Check server health

```powershell
Invoke-RestMethod "http://127.0.0.1:8080/health"
```

Do not attempt to open the root URL in a browser expecting a Web UI.

## Check loaded model

```powershell
Invoke-RestMethod "http://127.0.0.1:8080/v1/models" |
    ConvertTo-Json -Depth 10
```

## Check OpenCode configuration path

```powershell
notepad "$env:USERPROFILE\.config\opencode\opencode.json"
```

## Reload OpenCode configuration

```powershell
opencode reload
```

## Start DaisyRose

```powershell
Set-Location C:\AI
.\Start-DaisyRose.ps1
```

## Start Swift

```powershell
Set-Location C:\AI\rdna4-27b-inference
$modelPath = (Resolve-Path ".\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf").Path
.\server\launch.ps1 -Model $modelPath
```

---

# 19. USER WORKING PREFERENCES

The user requires technical accuracy and does not want invented settings or commands.

Important instructions for the next assistant:

1. Never guess exact configuration behaviour when it can be checked.
2. Distinguish confirmed facts from assumptions.
3. Use current official documentation and reliable sources for current software behaviour.
4. Cite factual claims derived from external sources.
5. For commands, be precise and explain what they do.
6. Preserve working configurations.
7. Make one change at a time.
8. Work interactively when performing system configuration changes:
   - Give one step.
   - Wait for the user's output.
   - Verify it.
   - Continue to the next step.
9. Do not ask for details already provided in this handoff.
10. The user prefers English responses, even when occasionally writing Greek.
11. Do not claim that a server crashed merely because an HTTP request timed out.
12. Do not claim a context test proves raw context retention when OpenCode has performed compaction.
13. Do not confuse the unavailable browser UI with the available PowerShell-accessible HTTP API.

---

# 20. NEXT STEP — DIRECT 69,467-TOKEN API TEST

**This is where the next conversation must resume.**

The user has agreed to perform a direct llama.cpp API request, bypassing OpenCode.

The intended test:

- Model: DaisyRose-27B-Q3_K_XL.gguf
- Runtime context: 98,304
- KV cache: F16 K / F16 V
- KV GPU offload: disabled
- Server port: 8080
- Test document: `C:\AI\daisyrose-96k-context-test.txt`
- Exact document token count: 69,467
- Single inference request.
- No OpenCode compaction.
- Five retrieval questions.
- Measure prompt processing, generation speed, total time and memory.

First, run a preflight using PowerShell:

```powershell
$base = "http://127.0.0.1:8080"
$document = "C:\AI\daisyrose-96k-context-test.txt"

Write-Host "`n===== SERVER HEALTH =====" -ForegroundColor Cyan
Invoke-RestMethod "$base/health" | ConvertTo-Json

Write-Host "`n===== LOADED MODEL =====" -ForegroundColor Cyan
Invoke-RestMethod "$base/v1/models" | ConvertTo-Json -Depth 10

Write-Host "`n===== SERVER SLOTS =====" -ForegroundColor Cyan
Invoke-RestMethod "$base/slots" | ConvertTo-Json -Depth 10

Write-Host "`n===== TEST DOCUMENT =====" -ForegroundColor Cyan
Get-Item $document | Select-Object FullName, Length

Write-Host "`n===== AVAILABLE MEMORY =====" -ForegroundColor Cyan
Get-CimInstance Win32_OperatingSystem |
    Select-Object @{
        Name = "FreeRAM_GiB"
        Expression = {[math]::Round($_.FreePhysicalMemory / 1MB, 2)}
    } | Format-List
```

Do not proceed to the actual request until the preflight output has been checked.

After that, prepare a PowerShell script that submits the full document in a single POST to `/v1/chat/completions`, with a generous client timeout and explicit generation settings.

The direct request should bypass OpenCode completely.

**The ultimate objective is to determine whether DaisyRose can genuinely process a 69,467-token input in one uninterrupted inference request at 96K F16 context, without automatic conversation compaction.**

Do not change the working 96K baseline until this test has been completed and evaluated.