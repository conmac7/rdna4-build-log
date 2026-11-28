# SESSION HANDOFF SUMMARY
## Local AI Infrastructure — RDNA4, llama.cpp, OpenCode, DaisyRose 27B and Swift 1.5 27B

**Last updated:** November 28, 2026  
**Operating system:** Windows 11  
**Primary hardware:** AMD Ryzen 7 5700X / Radeon RX 9070 XT 16GB / 32GB DDR4-3200  
**Project location:** `C:\AI`

---

# 1. PROJECT PURPOSE AND OBJECTIVES

This project builds and maintains a local AI environment for software development and technical documentation.

The infrastructure uses:

- Windows 11.
- Custom llama.cpp builds.
- AMD ROCm/HIP.
- GGUF language models.
- OpenCode.
- Local OpenAI-compatible HTTP APIs.
- PowerShell automation.

The user owns three language models and has decided to assign them different responsibilities.

| Task | Model | Intended responsibility |
|---|---|---|
| Task 1 | DaisyRose-27B | Technical documentation, writing, explanations and documentation quality |
| Task 2 | Swift 1.5 / Qwen3.8-27B | Programming, software development, architecture and implementation |
| Secondary | Ternary Bonsai 2.5 27B | Currently not a priority |

The user prioritizes:

1. Output accuracy and quality.
2. Numerical precision.
3. Reliable operation.
4. Context capacity.
5. Inference speed.

The user accepts slower inference when it produces better results.

The user is willing to use system RAM to accommodate large KV caches, accepting reasonable performance trade-offs.

The goal is not merely to run local LLMs. It is to create a reliable and reproducible local development and documentation environment.

## Critical hardware constraint: ONE MODEL AT A TIME

The user has 32GB system RAM and 16GB GPU VRAM.

With the current model configurations, **DaisyRose and Swift cannot run simultaneously**.

The user operates exclusively with one active model:

- DaisyRose loaded means Swift is not loaded.
- Swift loaded means DaisyRose is not loaded.
- The user switches between them by stopping the current server and starting the other.

Never suggest that these models can normally operate simultaneously.

This is an explicitly established and important operational constraint.

---

# 2. HARDWARE

| Component | Configuration |
|---|---|
| CPU | AMD Ryzen 7 5700X |
| CPU cores | 8 cores / 16 threads |
| GPU | Gigabyte Radeon RX 9070 XT GAMING OC |
| GPU architecture | RDNA4 |
| GPU memory | 16GB GDDR6 |
| System RAM | 32GB DDR4-3200 |
| Motherboard | Gigabyte B550 AORUS Elite V2 rev. 1.2 |
| OS | Windows 11 |

Other software details:

- PowerShell.
- OpenCode.
- Custom llama.cpp ROCm/HIP builds.
- AMD GPU drivers.
- Python virtual environments.

The exact currently installed Adrenalin driver version was not verified in this session.

---

# 3. DIRECTORY STRUCTURE

Main directory:

`C:\AI`

The following directory structure was established from user-provided screenshots.

```text
C:\AI\
│
├── DaisyRose-27B\
│   ├── logs\
│   ├── models\
│   ├── llama.cpp\
│   └── rocm-runtime\
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

`rocm72-runtime` appears to be a Python virtual environment.

DaisyRose separately references:

`C:\AI\DaisyRose-27B\rocm-runtime`

Do not confuse these two runtime directories.

The exact packages inside the virtual environments have not all been inventoried.

---

# 4. EXACT MODEL INVENTORY

There are THREE independent language models, not four.

## 4.1 DaisyRose 27B

Exact filename:

`DaisyRose-27B-Q3_K_XL.gguf`

Full path:

`C:\AI\DaisyRose-27B\models\DaisyRose-27B-Q3_K_XL.gguf`

Official repositories:

- https://huggingface.co/RadixArk/DaisyRose-27B-GGUF
- https://huggingface.co/RadixArk/DaisyRose-27B-GGUF/blob/main/DaisyRose-27B-Q3_K_XL.gguf
- https://huggingface.co/RadixArk/DaisyRose-27B

Verified model metadata returned by llama.cpp:

| Property | Value |
|---|---:|
| Parameters | 27,320,697,856 |
| File size | 13,212,073,984 bytes |
| Quantization | Q3_K - Medium |
| Vocabulary | 248,320 |
| Embedding dimension | 5,120 |
| Training context | 262,144 |
| Current tested runtime context | 98,304 |

This is the user's Task 1 documentation model.

## 4.2 Swift 1.5 / Qwen3.8-27B

Exact filename:

`Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Full path:

`C:\AI\rdna4-27b-inference\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf`

Official repository:

https://huggingface.co/Safasfly/Swift-1.5-Qwen3.8-27B-GSQ-RCO-IQ3_S-P2-MTP1-GGUF

Exact GGUF:

https://huggingface.co/Safasfly/Swift-1.5-Qwen3.8-27B-GSQ-RCO-IQ3_S-P2-MTP1-GGUF/blob/main/Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf

Swift is paired specifically with the custom `rdna4-27b-inference` project.

It is the user's Task 2 programming and software-development model.

Its OpenCode configuration declares:

- Context: 262,144 tokens.
- Output: 16,384 tokens.
- Default reasoning: xhigh.
- Original additional reasoning variant: low.

A medium reasoning variant was subsequently planned.

## 4.3 Ternary Bonsai 2.5 27B

Exact language-model filename:

`Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf`

Exact multimodal projector:

`Ternary-Bonsai-2.5-27B-mmproj-Q8_0.gguf`

The projector is not a fourth language model.

Screenshot directory:

`C:\AI\Bonsai-demo\models\bonsai25.gguf\27B`

The OpenCode configuration supplied earlier references:

`C:\AI\Bonsai-demo\models\bonsai25-gguf\27B\Ternary-Bonsai-2.5-27B-PQ2_0-mtp.gguf`

There is a possible directory-name discrepancy between `bonsai25.gguf` and `bonsai25-gguf`.

Verify the actual path before modifying anything.

The user is not currently interested in optimizing Bonsai.

---

# 5. CUSTOM RDNA4 LLAMA.CPP BUILD

Main repository:

https://github.com/lnieuwenhuis/rdna4-27b-inference

User's build-log repository:

https://github.com/conmac7/rdna4-build-log

The custom build targets RDNA4 / RX 9070 XT.

## 5.1 Historical build failure and solution

The original build failed during embedded Web UI asset provisioning.

The build log showed:

- The matching `b1` UI checksum download failed.
- The build system downloaded the `latest` UI bundle as a fallback.
- The newer bundle used a different `_app/immutable/` structure.
- The older pinned llama.cpp source expected:
  - `bundle[hash].js`
  - `bundle[hash].css`
  - `workbox[hash].js`

The required assets were therefore missing, and CMake stopped the build.

The solution was to add:

`-DLLAMA_USE_PREBUILT_UI=OFF`

to the CMake configuration arguments.

This was necessary even though `LLAMA_BUILD_UI` was already OFF, because prebuilt UI asset provisioning was controlled by a separate option.

The relevant UI build cache was cleaned and the build was repeated successfully.

**The build completed successfully.**

## 5.2 Important distinction: Web UI versus HTTP API

The embedded llama.cpp Web UI is intentionally disabled.

The user cannot use the browser-based llama.cpp interface.

Do not instruct the user to open:

`http://127.0.0.1:8080`

expecting the Web UI.

However, the HTTP API works.

Successfully tested from PowerShell:

- `/health`
- `/v1/models`
- `/tokenize`
- `/slots`
- `/v1/chat/completions`

The DaisyRose server also uses `--no-ui`.

**Missing browser UI does not mean the HTTP API is unavailable.**

All future server interaction should use PowerShell and HTTP API requests.

---

# 6. LLAMA.CPP BUILD VERSION AND FEATURES

The DaisyRose executable reported:

```text
version: 0.7.0-dev (build 1, commit b540a4b)
built with Clang 24.0.0 for Windows AMD64
```

The user inspected the complete local `llama-server.exe --help` output.

It supports:

- F16, BF16, F32 and quantized KV-cache types.
- Flash Attention.
- No-KV-offload mode.
- Experimental adaptive KV streaming.
- Configurable GPU-resident KV memory pools.
- Various speculative decoding options.
- Jinja chat templates.
- Reasoning effort parameters.
- OpenAI-compatible API endpoints.

## 6.1 KV-cache defaults

The actual executable's help declares:

```text
--cache-type-k default: f16
--cache-type-v default: f16
```

The current DaisyRose launcher does not override them.

Therefore:

| Cache | Current setting |
|---|---|
| K | F16 |
| V | F16 |

This matches the user's quality-first preference.

## 6.2 `--no-kv-offload`

The DaisyRose launcher includes:

```powershell
"--no-kv-offload",
```

This disables KV-cache GPU offloading.

It does not disable the KV cache itself.

The configuration uses host-side processing and memory instead of offloading the KV cache to the GPU.

The user accepts slower inference to preserve the preferred memory arrangement and precision.

## 6.3 Adaptive KV streaming

The DaisyRose executable recognizes:

- `--kv-stream`
- `--kv-resident-mib`
- `--kv-resident-margin-mib`
- `--kv-resident-mib-decode`
- `--kv-stream-split`

However, its own help explicitly states that `--kv-stream` requires:

1. Flash Attention enabled (`-fa on`).
2. K cache = `q8_0`.
3. V cache = `q4_0`.

Therefore, this streaming implementation is NOT compatible with the user's current F16/F16 configuration.

The RDNA4 project has a documented adaptive streaming configuration using:

```powershell
"-fa", "on",
"-ctk", "q8_0",
"-ctv", "q4_0",
"--kv-stream",
"--kv-resident-mib", "1355",
"--kv-stream-split"
```

The project also documents `--load-mode none` in its deployment configuration.

This implementation is an alternative if the user later prioritizes context capacity over full F16 KV precision.

It has NOT been applied to DaisyRose.

Do not suggest combining `--kv-stream` with F16 K and F16 V on this build.

## 6.4 Precision comparison

F16 has greater numerical representational precision than Q8_0.

However, a previous Qwen3.8-27B KV quantization investigation found Q8 results close to F16 in certain measurements, and in some limited metrics marginally better.

That does NOT establish Q8 as generally more accurate.

The user's current decision is to retain F16/F16 because quality and precision take priority over memory savings.

---

# 7. DAISYROSE STARTUP SCRIPT

The user has:

`C:\AI\Start-DaisyRose.ps1`

The user successfully ran this script.

Its current context setting is 98,304.

Complete script as supplied by the user:

```powershell
$ErrorActionPreference = "Stop"

$root = "C:\AI\DaisyRose-27B"
$bin = "$root\llama.cpp\bin"
$coreBin = "$root\rocm-runtime\_rocm_sdk_core\bin"
$libsBin = "$root\rocm-runtime\_rocm_sdk_libraries\bin"
$model = "$root\models\DaisyRose-27B-Q3_K_XL.gguf"

$port = 8080

$env:PATH = "$coreBin;$libsBin;$bin;$env:PATH"

if (Get-NetTCPConnection -LocalPort $port -State Listen -ErrorAction SilentlyContinue) {
    throw "Port 8080 is already occupied."
}

Write-Host "`n===== STARTING DAISYROSE =====" -ForegroundColor Cyan

$arguments = @(
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
)

$process = Start-Process `
    -FilePath "$bin\llama-server.exe" `
    -ArgumentList $arguments `
    -WorkingDirectory $bin `
    -PassThru `
    -RedirectStandardOutput "$root\logs\opencode-stdout.log" `
    -RedirectStandardError "$root\logs\opencode-stderr.log"

Write-Host "PID: $($process.Id)"

$deadline = (Get-Date).AddMinutes(15)
$ready = $false

while ((Get-Date) -lt $deadline) {

    $process.Refresh()

    if ($process.HasExited) {
        Get-Content "$root\logs\opencode-stderr.log" -Tail 60
        throw "DaisyRose failed to start."
    }

    try {
        $health = Invoke-WebRequest `
            -Uri "http://127.0.0.1:8080/health" `
            -SkipHttpErrorCheck `
            -TimeoutSec 5

        if ($health.StatusCode -eq 200) {
            $ready = $true
            break
        }
    } catch {}

    Start-Sleep -Seconds 3
}

if (!$ready) {
    throw "DaisyRose did not become ready."
}

Write-Host "`nDaisyRose is ready!" -ForegroundColor Green

Invoke-RestMethod "http://127.0.0.1:8080/v1/models" |
    ConvertTo-Json -Depth 5
```

## 7.1 DaisyRose server settings

| Setting | Value |
|---|---|
| Model | DaisyRose-27B-Q3_K_XL |
| GPU layers | 999 |
| Context | 98,304 |
| Parallel slots | 1 |
| KV GPU offload | Disabled |
| K cache | F16 default |
| V cache | F16 default |
| Host | 127.0.0.1 |
| Port | 8080 |
| Web UI | Disabled |
| Jinja | Enabled |
| Temperature | 1.0 |
| Top P | 0.95 |
| Top K | 20 |
| Min P | 0.0 |

## 7.2 DaisyRose startup

```powershell
Set-Location C:\AI
.\Start-DaisyRose.ps1
```

---

# 8. DAISYROSE SHUTDOWN SCRIPT

A dedicated script was generated:

`Stop-DaisyRose.ps1`

Download reference from the previous session:

`Stop-DaisyRose.ps1` was delivered as a downloadable artifact.

The user was instructed to save it to:

`C:\AI\Stop-DaisyRose.ps1`

**The user has not yet explicitly confirmed that the file was copied into C:\AI or tested.**

The generated script:

1. Checks port 8080.
2. Identifies the listening process.
3. Verifies the executable path is exactly:

   `C:\AI\DaisyRose-27B\llama.cpp\bin\llama-server.exe`

4. Verifies the command line contains the expected DaisyRose GGUF path.
5. Queries `/slots` to determine whether inference is active.
6. If inference is active, warns the user and requires them to type `STOP`.
7. If slot status cannot be queried, requests explicit confirmation.
8. Stops the verified DaisyRose process.
9. Verifies the process exits and port 8080 is released.

The script uses `Stop-Process -Force` only after verifying the process identity and, when possible, checking active inference status.

Usage:

```powershell
Set-Location C:\AI
.\Stop-DaisyRose.ps1
```

It does not target arbitrary processes.

---

# 9. EXACT SWIFT STARTUP PROCEDURE

The user's personal startup sequence for Swift is:

```powershell
Set-Location C:\AI\rdna4-27b-inference

$modelPath = (Resolve-Path ".\models\Swift-1.5-P2-GSQ-RCO-IQ3_S-mtp1.gguf").Path

.\server\launch.ps1 -Model $modelPath
```

This exact procedure must be preserved.

Do not replace it with a generic startup command.

Swift OpenAI-compatible endpoint:

`http://127.0.0.1:18080/v1`

The user operates one model at a time.

---

# 10. OPEN CODE CONFIGURATION

Main configuration:

`C:\Users\pgali\.config\opencode\opencode.json`

OpenCode version observed using `opencode --version`:

`opencode v2.3.2`

The configuration directory also contained:

- `opencode-bonsai.json`
- `opencode.json`
- `service.json`

The active configuration is `opencode.json`.

A previous backup was created:

`opencode.json.backup-20261127-180128`

## 10.1 Initial configuration versus current configuration

The initial configuration, referred to as `init`, used:

```json
"model": "bonsai/bonsai25"
```

The expanded configuration, referred to as `current`, changed this to:

```json
"model": "swift/swift15"
```

This changes the default model to Swift.

It does NOT remove Bonsai or the other configured models.

## 10.2 Provider and model identifiers

There are three providers and three model entries:

| Provider ID | Model ID | Actual LLM |
|---|---|---|
| `bonsai` | `bonsai25` | Ternary Bonsai 2.5 |
| `swift` | `swift15` | Swift 1.5 |
| `daisyrose` | `daisyrose27b` | DaisyRose 27B |

There is only one Swift model.

In:

```json
"swift": {
    "models": {
        "swift15": {
        }
    }
}
```

`swift` is the provider identifier and `swift15` is the model identifier.

The default model:

`swift/swift15`

## 10.3 Provider endpoints

| Model | Endpoint |
|---|---|
| Swift | `http://127.0.0.1:18080/v1` |
| DaisyRose | `http://127.0.0.1:8080/v1` |
| Bonsai | `http://127.0.0.1:8080/v1` |

Bonsai and DaisyRose share port 8080 in the configuration.

The user does not operate multiple models simultaneously.

## 10.4 Model limits

Original/current intended values:

| Model | Context | Output |
|---|---:|---:|
| Swift | 262,144 | 16,384 |
| DaisyRose | 98,304 | 16,384 |
| Bonsai | 131,072 | 16,384 |

DaisyRose's active context value was explicitly changed and verified at 98,304.

## 10.5 Sampling parameters

The intended configuration for DaisyRose and Swift uses:

```json
"temperature": 1.0,
"top_p": 0.95,
"top_k": 20,
"min_p": 0.0
```

These are the model-author-recommended values investigated during the previous session.

DaisyRose's server launcher explicitly applies all four values.

Swift's original OpenCode configuration did not explicitly specify them in its `body`.

A replacement configuration file was generated to make Swift's sampling settings equally explicit.

That generated replacement also exposed medium and low reasoning variants for Swift and medium for DaisyRose.

**Important verification status:**

The user subsequently verified the active DaisyRose context and header timeout, but did not paste the entire final active OpenCode JSON again.

Therefore:

- DaisyRose context 98,304: confirmed active.
- DaisyRose headerTimeout 1,800,000: confirmed active.
- Swift explicit sampling body: generated in the replacement file, but final active status not independently verified.
- Swift medium/low variants: generated in replacement file, but final active status not independently verified.
- DaisyRose medium variant: generated in replacement file, but final active status not independently verified.

Do not assume the downloadable replacement file is identical to the user's final active configuration.

In fact, the generated copy still contained DaisyRose context 65,536 and did not include the subsequently added `headerTimeout`.

It must NOT be copied over the active file unchanged.

---

# 11. MODEL RECOMMENDATIONS AND REASONING

The official model repositories were inspected.

Both model configurations were aligned around:

| Parameter | Intended value |
|---|---:|
| Temperature | 1.0 |
| Top P | 0.95 |
| Top K | 20 |
| Min P | 0.0 |
| Default reasoning | xhigh |

DaisyRose's developer recommends xhigh and provides reasoning alternatives.

Swift's model card also documents the sampling configuration and reasoning effort support.

The user prioritizes quality and therefore uses xhigh by default.

## 11.1 Reasoning-format clarification

The user noticed that the llama.cpp slot diagnostics reported:

```json
"reasoning_format": "deepseek"
```

The user correctly questioned why a Qwen-based DaisyRose model would use a DeepSeek format.

Investigation of the exact llama.cpp commit `b540a4b` showed that the term refers to an **API response format**, not the model architecture or its chat template.

The source code explicitly distinguishes:

- The reasoning format of the API response.
- The reasoning format used by the model's chat template.

The enum in `common/common.h` includes:

- `none`
- `auto`
- `deepseek`
- `deepseek-legacy`

The source comment explains that `auto` uses the same API field arrangement as DeepSeek, namely `message.reasoning_content`.

There is no `qwen` reasoning-format option in this executable.

The actual DaisyRose model remains Qwen3.8-based.

Its chat template uses:

```text
<think>
Reasoning content
</think>

Final answer
```

The launcher does not explicitly set:

`--reasoning-format deepseek`

The default is `auto`, but the runtime slot reports `deepseek` as the resolved API convention.

The successful direct API response contained:

- `message.reasoning_content`
- `message.content`

Both fields were populated.

**Decision:** Do not change the setting to `qwen`. That is not a supported value in this build. The current API reasoning-field arrangement works correctly.

The user's model is DaisyRose-27B, not DeepSeek.

## 11.2 Important OpenCode compatibility

The DaisyRose and Swift OpenCode configurations use:

```json
"compatibility": {
    "reasoningField": "reasoning_content"
}
```

This matches the local server's response format.

---

# 12. DAISYROSE CONTEXT CAPACITY INVESTIGATION

The user wanted to know whether DaisyRose could use a larger context while retaining F16 K/V and `--no-kv-offload`.

## 12.1 Architecture findings

The previous investigation established that the underlying Qwen3.8-27B architecture uses a hybrid attention design:

- 64 layers.
- 48 Gated DeltaNet layers.
- 16 full-attention layers.
- 4 KV heads.
- Head dimension 256.

Only the full-attention layers require a conventional KV cache that grows with context length.

For F16 K/V, the estimated cache size is:

```text
16 full-attention layers
× 2 (K and V)
× 4 KV heads
× 256 head dimensions
× 2 bytes
= 65,536 bytes/token
```

That is approximately 64 KiB/token.

Estimated F16 K/V cache requirements:

| Context | Estimated cache |
|---|---:|
| 64K | 4 GiB |
| 96K | 6 GiB |
| 128K | 8 GiB |
| 192K | 12 GiB |
| 256K | 16 GiB |

These are architectural estimates, not direct measurements of all runtime allocations.

## 12.2 Baseline at 64K

The user's actual measurements with DaisyRose loaded at 65,536 context:

| Measurement | Result |
|---|---:|
| Total RAM | 31.93 GiB |
| Free RAM | 6.22 GiB |
| Working set | 16.01 GiB |
| Private memory | 16.62 GiB |

## 12.3 Changed runtime context to 96K

The user changed the launcher:

From:

```powershell
"-c", "65536",
```

To:

```powershell
"-c", "98304",
```

DaisyRose successfully started.

The `/v1/models` response reported:

```json
"n_ctx": 98304,
"n_ctx_train": 262144
```

Thus, runtime context is 98,304 and training context is 262,144.

## 12.4 Corrected 96K memory measurements

The first measurement was incorrect because another application was open.

The corrected values:

| Measurement | 64K | 96K |
|---|---:|---:|
| Free RAM | 6.22 GiB | 6.66 GiB |
| Working set | 16.01 GiB | 18.05 GiB |
| Private memory | 16.62 GiB | 18.66 GiB |

Working set and private memory increased by approximately 2.04 GiB.

This closely matches the estimated extra F16 KV cache for the additional 32K tokens.

It is consistent with the architecture calculation, but private memory is not a direct KV-cache measurement.

## 12.5 Simple inference test at 96K

A short question was sent through OpenCode:

"Explain the difference between RAM and VRAM in exactly three sentences."

DaisyRose returned exactly three coherent sentences.

Reported performance from the screenshot:

- 7.4 tokens/sec.
- 1 minute 27 seconds.

This confirmed basic inference at 96K.

---

# 13. LONG-CONTEXT SYNTHETIC TEST DOCUMENT

A synthetic technical archive was created:

`C:\AI\daisyrose-96k-context-test.txt`

The final file has:

| Property | Value |
|---|---:|
| Lines | 1,606 |
| Numbered records | 1,600 |
| Characters | 257,192 |
| Document tokens | 69,467 |

The file was generated with PowerShell and tokenized through:

`http://127.0.0.1:8080/tokenize`

An initial version produced 121,517 tokens and was too large.

A corrected generator reduced it to 1,600 records and produced the final 69,467-token archive.

## 13.1 Verification checkpoints

The document contains:

Beginning:

`MAPLE-ORCHID-4817`

Middle:

`COPPER-DELTA-9352`

End:

`VIOLET-HARBOR-6024`

Record 01600 checksum formula:

```text
((record_number * 7919 + 104729) % 999983)
```

For record 01600:

`775333`

Total numbered records:

`1600`

These values are independently verifiable using the generator.

---

# 14. FIRST LONG-CONTEXT EXPERIMENT: OPENCODE TOOL READING

The first test attempted to read the document using nine sequential OpenCode file-reading operations.

The ranges were:

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

## 14.1 First attempt

OpenCode returned:

`Timed out waiting for response headers`

DaisyRose itself remained healthy.

Logs showed generation continuing at approximately 3.62 tokens/sec.

A server health check returned `ok`.

There was no evidence of a model-server crash.

## 14.2 OpenCode timeout adjustment

The DaisyRose provider configuration was changed to:

```json
"settings": {
  "baseURL": "http://127.0.0.1:8080/v1",
  "headerTimeout": 1800000
}
```

This is 30 minutes.

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

## 14.3 Second attempt

The nine-range test completed.

However, during the transition from range 7 to range 8, OpenCode automatically compacted the conversation.

The screenshot showed:

`37.7K in · 5.8K out`

After compaction the displayed context indicator showed approximately:

`11K`

DaisyRose continued reading the remaining ranges.

It returned the correct five answers.

| Test | Result |
|---|---|
| BEGIN checkpoint | Correct |
| MIDDLE checkpoint | Correct |
| END checkpoint | Correct |
| Record 01600 checksum | Correct |
| Record count | Correct |

Reported screenshot performance:

- Approximately 5.1 tokens/sec.
- Approximately 43 minutes 23 seconds total.

This test demonstrated that the model could complete a multi-step document retrieval process and continue successfully after OpenCode compaction.

It did NOT prove that all 69,467 original document tokens were simultaneously retained in the raw model context.

OpenCode compaction summarizes/replaces older conversation content.

This distinction led to the next, cleaner experiment.

---

# 15. OPEN CODE MEMORY INVESTIGATION

A preflight check before the direct API test showed only 1.63 GiB free system RAM.

The user asked how to stop OpenCode.

The following processes were identified:

```text
PID 15356
opencode-cli.exe
C:\Users\pgali\AppData\Roaming\ai.opencode.desktop\cli\2.4.2-x64\opencode-cli.exe serve --service
```

```text
PID 7436
opencode-pty.exe
C:\Users\pgali\.cache\opencode\bin\opencode-pty\0.6.0-3ca87f140ff90483\opencode-pty.exe daemon
```

Notepad++ was also running but was not stopped:

```text
PID 9644
notepad++.exe
```

The user ran:

```powershell
Stop-Process -Id 15356,7436 -Force
```

After closing the OpenCode background processes, available system RAM increased to:

`7.380 GiB`

This is an observed correlation, not an exact allocation measurement of either process.

It is important because OpenCode can consume additional RAM during long-context workflows.

A version discrepancy was also observed:

- PowerShell `opencode --version` reported v2.3.2.
- The desktop-bundled service executable path contained `2.4.2-x64`.

These may be separate bundled/standalone versions. Verify if relevant.

---

# 16. SECOND EXPERIMENT: DIRECT LLAMA.CPP API REQUEST

This is the most important completed experiment.

The goal was to eliminate:

- OpenCode tool-call overhead.
- OpenCode file-reading operations.
- OpenCode automatic compaction.
- Multi-request orchestration.

Instead, the complete document was sent directly to DaisyRose in ONE HTTP request.

Endpoint:

`http://127.0.0.1:8080/v1/chat/completions`

No browser UI was involved.

## 16.1 Preflight

The following values were verified:

| Check | Result |
|---|---|
| Server health | `ok` |
| Loaded model | DaisyRose Q3_K_XL |
| Runtime context | 98,304 |
| Training context | 262,144 |
| Slot | Idle |
| Test document | 257,192 bytes |

The slot diagnostic also showed:

```json
"temperature": 1.0,
"top_k": 20,
"top_p": 0.949999988079071,
"min_p": 0.0
```

It reported:

```json
"reasoning_format": "deepseek",
"reasoning_in_content": false,
"generation_prompt": "<|im_start|>assistant\n<think>\n"
```

This is an API reasoning-format label, not a DeepSeek model identity.

The user closed OpenCode before submitting the large request.

## 16.2 Request preparation

A preparation script was created:

`C:\AI\Prepare-DaisyRose-DirectTest.ps1`

It:

1. Checked the server health.
2. Queried the loaded model.
3. Confirmed context 98,304.
4. Checked that the server slot was idle.
5. Checked available RAM.
6. Read the entire test document.
7. Created system and user prompts.
8. Tokenized the complete input through `/tokenize`.
9. Verified context headroom.
10. Saved the final request JSON.

Prepared request:

`C:\AI\daisyrose-direct-test-request.json`

Actual preparation results:

| Measurement | Result |
|---|---:|
| Document characters | 257,192 |
| Document alone | 69,467 tokens |
| Full prepared input | 69,626 tokens |
| Context capacity | 98,304 |
| Output allowance | 1,024 |
| Remaining estimated headroom | 27,654 |
| Available RAM at preparation | 5.01 GiB |

The API later reported 69,678 prompt tokens. This is slightly higher than the preparation tokenization result, consistent with additional request/chat-format overhead. The exact source of the 52-token difference was not separately isolated.

## 16.3 Direct execution script

A second script was created:

`C:\AI\Run-DaisyRose-DirectTest.ps1`

It:

- Rechecked server health.
- Checked the slot was idle.
- Checked available RAM.
- Loaded the prepared request JSON.
- Submitted one non-streaming POST request.
- Used a 7,200-second client timeout.
- Saved the response to JSON.
- Printed reasoning and final content separately.
- Printed usage and timing information.
- Captured post-inference memory.

The request parameters were:

| Parameter | Value |
|---|---|
| Model | Exact DaisyRose ID from `/v1/models` |
| Temperature | 1.0 |
| Top P | 0.95 |
| Top K | 20 |
| Min P | 0.0 |
| Reasoning effort | xhigh |
| Maximum output | 1,024 tokens |
| Stream | false |
| Cache prompt | false |
| Timings per token | true |
| Client timeout | 7,200 seconds |

The full user message contained the entire test archive.

The system prompt instructed the model to treat the archive as data, not as instructions.

No OpenCode session was involved.

## 16.4 Actual execution result

The direct request completed successfully.

Elapsed time:

`00:06:43.1861145`

The response was saved at:

`C:\AI\daisyrose-direct-test-response.json`

### API usage

| Field | Value |
|---|---:|
| Prompt tokens | 69,678 |
| Completion tokens | 409 |
| Total tokens | 70,087 |
| Cached prompt tokens | 0 |
| Finish reason | `stop` |

### Actual server timings

| Metric | Value |
|---|---:|
| Prompt processing time | 214,798.507 ms |
| Prompt processing speed | 324.387729566482 tokens/sec |
| Generation time | 187,518.798 ms |
| Generation speed | 2.17578186481336 tokens/sec |
| Generated tokens | 409 |
| Total wall time | 6 minutes 43.186 seconds |

The sum of prompt and generation timing is approximately 402.32 seconds; total wall time was approximately 403.19 seconds.

### Memory

| Measurement | Result |
|---|---:|
| Free RAM before actual request | 7.26 GiB |
| Free RAM afterward | 13.74 GiB |

The available RAM measurements are snapshots. Do not assume the difference represents an exact amount of RAM released by inference.

## 16.5 Final model answers

DaisyRose correctly returned all five verification values:

1. BEGIN CHECKPOINT: `MAPLE-ORCHID-4817`
2. MIDDLE CHECKPOINT: `COPPER-DELTA-9352`
3. END CHECKPOINT: `VIOLET-HARBOR-6024`
4. Record 01600 checksum: `775333`
5. Numbered records: `1600`

All five answers were correct.

The saved JSON was subsequently inspected.

Verified:

- `finish_reason`: `stop`
- `message.content` exists and is not empty.
- `message.reasoning_content` exists.
- Usage reports 69,678 input tokens.
- Timings object is present.

A PowerShell diagnostic attempted to print the final content length but had a string-interpolation mistake. It printed the actual content followed by `.Length`.

That was a diagnostic-script issue, not a model response problem.

## 16.6 What this definitively proves

This is the cleanest long-context result obtained so far.

It demonstrates that DaisyRose successfully processed approximately 69.7K prompt tokens in ONE uninterrupted request using:

- Runtime context 98,304.
- F16 K cache.
- F16 V cache.
- `--no-kv-offload`.
- xhigh reasoning.
- No OpenCode involvement.
- No OpenCode automatic compaction.
- No cached prompt tokens.

It returned all five correct answers and completed normally.

**The 96K F16/F16 configuration has passed the direct long-context validation.**

The direct request was not a multi-chunk workflow.

No OpenCode compaction occurred.

This is the key successful result of the entire investigation.

---

# 17. CURRENT DAISYROSE CONFIGURATION

The validated configuration is:

| Parameter | Current value |
|---|---|
| Model | DaisyRose-27B-Q3_K_XL |
| Runtime context | 98,304 |
| Model training context | 262,144 |
| K cache | F16 |
| V cache | F16 |
| KV GPU offload | Disabled |
| KV streaming | Disabled |
| GPU layers | 999 |
| Parallel slots | 1 |
| Temperature | 1.0 |
| Top P | 0.95 |
| Top K | 20 |
| Min P | 0.0 |
| Reasoning | xhigh |
| Server port | 8080 |
| Host | 127.0.0.1 |
| Web UI | Disabled |
| OpenCode header timeout | 1,800,000 ms |
| OpenCode context limit | 98,304 |
| OpenCode output limit | 16,384 |

The direct 69.7K-token test was successful with these settings.

**Decision:** Retain 96K context, F16/F16 cache and `--no-kv-offload`.

Do not increase to 128K or 256K without a new memory assessment and explicit reason.

---

# 18. OPEN CODE REASONING-FORMAT CLARIFICATION

The user questioned why the server reported `reasoning_format: deepseek` when DaisyRose is a Qwen-based model.

Investigation established:

- `deepseek` in llama.cpp is an API response parsing convention.
- It does not change model identity.
- It does not select a DeepSeek architecture.
- The Qwen3.8 Jinja chat template remains in use.
- There is no `qwen` value in the accepted `--reasoning-format` list.
- `auto` is the documented default.
- In this llama.cpp version, `auto` resolves to the same API field arrangement as `deepseek` for models using this reasoning format.

The current server response separates:

- Reasoning in `message.reasoning_content`.
- Final answer in `message.content`.

The direct API test confirmed both fields are populated correctly.

No change to the reasoning-format option is required.

---

# 19. STOP-DAISYROSE SCRIPT

A shutdown script was generated and delivered as a downloadable file:

`Stop-DaisyRose.ps1`

Intended destination:

`C:\AI\Stop-DaisyRose.ps1`

The user has not explicitly confirmed that it has been copied or executed.

Its logic is safety-oriented:

- Identify the process listening on port 8080.
- Verify its executable path.
- Verify the command line references the exact DaisyRose GGUF.
- Check `/slots` for an active request.
- Ask for explicit confirmation if inference is running.
- Stop the verified process.
- Confirm that the process exits and port 8080 is released.

Usage:

```powershell
Set-Location C:\AI
.\Stop-DaisyRose.ps1
```

Do not use a generic `Stop-Process -Name llama-server` command to stop the server. Target the verified DaisyRose process.

---

# 20. IMPORTANT TEST AND SCRIPT FILES

The following files were created or used during this investigation.

| File | Purpose |
|---|---|
| `C:\AI\Start-DaisyRose.ps1` | Starts DaisyRose |
| `C:\AI\Stop-DaisyRose.ps1` | Stops verified DaisyRose process; installation not confirmed |
| `C:\AI\Prepare-DaisyRose-DirectTest.ps1` | Prepares direct API request |
| `C:\AI\Run-DaisyRose-DirectTest.ps1` | Executes direct API inference |
| `C:\AI\daisyrose-96k-context-test.txt` | 69,467-token synthetic archive |
| `C:\AI\daisyrose-direct-test-request.json` | Prepared direct API request |
| `C:\AI\daisyrose-direct-test-response.json` | Successful direct API response |
| `C:\AI\command.ps1` | Reused for multiple diagnostics; current contents should not be assumed |

DaisyRose logs:

```text
C:\AI\DaisyRose-27B\logs\opencode-stdout.log
C:\AI\DaisyRose-27B\logs\opencode-stderr.log
```

User's GitHub build-log repository:

https://github.com/conmac7/rdna4-build-log

---

# 21. DECISIONS MADE AND REASONS

| Subject | Decision | Reason |
|---|---|---|
| Task 1 | DaisyRose | Documentation and technical writing |
| Task 2 | Swift 1.5 | Coding and software development |
| Default OpenCode model | Swift | Primary implementation model |
| Active model count | One at a time | 32GB RAM / 16GB VRAM constraint |
| DaisyRose runtime context | 98,304 | Successfully validated |
| DaisyRose K/V cache | F16/F16 | Maximum numerical representation precision |
| DaisyRose KV offload | Disabled | Current working memory arrangement |
| KV streaming | Not used | Requires Q8 K / Q4 V |
| Sampling | 1.0 / 0.95 / 20 / 0.0 | Model-author recommendations |
| Reasoning | xhigh | Quality-oriented default |
| DaisyRose header timeout | 30 minutes | Long inference response-header waits |
| Embedded Web UI | Disabled | Historical UI asset build problem |
| OpenCode tool-based 69K test | Not sufficient alone | Automatic compaction occurred |
| Direct API 69K test | Successful | Demonstrated one-request processing without compaction |

---

# 22. CONFIRMED FACTS VS. UNVERIFIED INFORMATION

## Confirmed

- Hardware is Ryzen 7 5700X, RX 9070 XT 16GB and 32GB RAM.
- Three language models are installed.
- Exact model filenames are known.
- DaisyRose's custom llama.cpp executable is version `0.7.0-dev`, commit `b540a4b`.
- DaisyRose successfully starts with 98,304 context.
- F16 K/V are the current default cache types.
- `--kv-stream` requires Flash Attention, Q8 K and Q4 V.
- OpenCode's active DaisyRose context was verified at 98,304.
- DaisyRose header timeout was verified at 1,800,000 ms.
- The 69,467-token archive was created and tokenized.
- The full direct request was processed in a single request.
- The API reported 69,678 prompt tokens and 409 completion tokens.
- All five verification answers were correct.
- The direct API request completed normally with `finish_reason=stop`.
- No OpenCode compaction occurred during the direct API request.
- DaisyRose's reasoning and final response fields were both populated.

## Not fully verified

- Whether the active OpenCode configuration contains every planned Swift body and reasoning variant change.
- Whether the generated `Stop-DaisyRose.ps1` has been copied to `C:\AI`.
- Whether the shutdown script has been tested.
- Whether 128K or 256K F16 context would be practical.
- Whether a future upgrade to 64GB RAM would justify a larger permanent context.
- Whether adaptive KV streaming would provide a worthwhile trade-off for DaisyRose.
- Exact package inventory of all runtime environments.
- Exact installed Adrenalin version.
- Whether the OpenCode desktop-bundled CLI version and standalone CLI version are intentionally different.

---

# 23. REMAINING POTENTIAL PROBLEMS

## 23.1 OpenCode compaction

The 69K multi-chunk OpenCode test triggered automatic compaction.

This is separate from llama.cpp's runtime context capacity.

The header timeout adjustment allowed the workflow to complete but does not disable compaction.

For future large-document tasks, use the direct API if the purpose is to validate raw single-request context processing.

For normal OpenCode workflows, be aware that automatic compaction can summarize previous conversation content.

## 23.2 Memory

Available memory changed significantly between snapshots.

Examples:

| Situation | Available RAM |
|---|---:|
| DaisyRose at 64K, earlier baseline | 6.22 GiB |
| DaisyRose at 96K, corrected startup snapshot | 6.66 GiB |
| During multi-chunk OpenCode investigation | 5.68 GiB |
| Before closing OpenCode background processes | 1.63 GiB |
| After closing OpenCode CLI and PTY | 7.38 GiB |
| Direct-test preparation | 5.01 GiB |
| Immediately before direct inference | 7.26 GiB |
| After direct inference | 13.74 GiB |

These measurements were taken at different times and are not perfectly comparable.

They demonstrate why memory should be checked immediately before large inference requests.

## 23.3 Output quality versus KV quantization

F16 is the preferred configuration.

Q8 cache measurements on related Qwen3.8 configurations were close to F16 in some tests, but no evidence established that Q8 is generally more accurate.

The user's choice remains F16/F16.

## 23.4 OpenCode configuration consistency

The active configuration has not been pasted in its entirety after every modification.

Before any future changes, inspect the actual file rather than relying on a previously generated replacement JSON.

The older generated replacement file still contained DaisyRose context 65,536 and lacked the subsequently added header timeout.

Do not overwrite the current active configuration with that stale file.

---

# 24. USER WORKING PREFERENCES

The user strongly prefers accuracy over speculation.

Requirements for future assistance:

1. Never invent technical settings, commands or configuration behaviour.
2. Verify uncertain or current software facts against reliable documentation.
3. Use official sources where possible.
4. Cite external factual claims.
5. Distinguish confirmed facts from assumptions.
6. Preserve existing working configurations.
7. Make one change at a time.
8. Work interactively when configuring Windows:
   - Give one step.
   - Wait for the PowerShell output.
   - Check the result.
   - Continue.
9. Do not repeat questions whose answers are already present here.
10. Always respect the one-model-at-a-time hardware constraint.
11. Do not assume that both DaisyRose and Swift can be loaded simultaneously.
12. Do not confuse the unavailable browser Web UI with the available PowerShell-accessible HTTP API.
13. Do not claim that a timeout means the inference server crashed without evidence.
14. Do not claim that an OpenCode tool-based test proves simultaneous raw context retention when automatic compaction occurred.

The user prefers English responses, including when occasionally writing in Greek.

---

# 25. NEXT STEP

**The 96K DaisyRose direct API validation is COMPLETE and SUCCESSFUL.**

There is no need to repeat the synthetic 69K direct test unless a configuration change requires revalidation.

The next useful project stage is to move from synthetic validation to practical model evaluation.

Recommended sequence:

### Task 1 — Real documentation workload

Use DaisyRose at its validated 98,304-token configuration.

Evaluate it on a realistic documentation task involving actual technical material, with criteria such as:

- Technical accuracy.
- Completeness.
- Structure and organization.
- Consistency.
- Ability to follow detailed requirements.
- Correct handling of long source material.
- Hallucination resistance.

### Task 2 — Coding workload

Stop DaisyRose before starting Swift.

Use the user's exact Swift startup sequence and evaluate actual software implementation tasks.

Preserve the established Task 2 evaluation context and previous benchmark decisions.

### Immediate operational housekeeping

Before the next major benchmark, confirm whether:

`C:\AI\Stop-DaisyRose.ps1`

exists and is usable.

Do not change the validated DaisyRose context, F16/F16 cache arrangement or sampling configuration without a specific reason.

**The established baseline is DaisyRose 27B, 96K context, F16 K/V, no KV offload, xhigh reasoning, and successful one-request processing of 69,678 prompt tokens in 6 minutes 43 seconds, with all five retrieval checks correct.**

This is the baseline to preserve going forward.