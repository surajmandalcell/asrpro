# ASR Pro Performance Report

**Audio:** 158.4s (2.6min) | **Generated:** 06:27:46 | **Duration:** 69.3s

## Results

| Model | Requested->Actual | Load | Transcribe | RTF | WPS | Status |
|-------|--------------------|------|------------|-----|-----|--------|
| whisper-tiny_detected | ERROR | - | - | - | - | FAIL |
| whisper-tiny_cpu | ERROR | - | - | - | - | FAIL |
| whisper-tiny_cuda | ERROR | - | - | - | - | FAIL |
| whisper-tiny_vulkan | ERROR | - | - | - | - | FAIL |
| whisper-tiny-local_detected | ERROR | - | - | - | - | FAIL |
| whisper-tiny-local_cpu | ERROR | - | - | - | - | FAIL |
| whisper-tiny-local_cuda | ERROR | - | - | - | - | FAIL |
| whisper-tiny-local_vulkan | ERROR | - | - | - | - | FAIL |
| whisper-base | detected->cuda | 2.4s | 1.9s | 83.5x | 177 | PASS |
| whisper-base | cpu->cpu | 0.7s | 1.5s | 104.8x | 222 | PASS |
| whisper-base | cuda->cuda | 1.0s | 1.5s | 106.8x | 227 | PASS |
| whisper-base_vulkan | vulkan->cpu | 0.7s | 1.5s | 104.0x | 221 | PASS |
| whisper-large_detected | ERROR | - | - | - | - | FAIL |
| whisper-large_cpu | ERROR | - | - | - | - | FAIL |
| whisper-large_cuda | ERROR | - | - | - | - | FAIL |
| whisper-large_vulkan | ERROR | - | - | - | - | FAIL |
| whisper-base-local_detected | ERROR | - | - | - | - | FAIL |
| whisper-base-local_cpu | ERROR | - | - | - | - | FAIL |
| whisper-base-local_cuda | ERROR | - | - | - | - | FAIL |
| whisper-base-local_vulkan | ERROR | - | - | - | - | FAIL |
| parakeet-tdt-0.6b-v2 | detected->cuda | 3.5s | 9.8s | 16.1x | 186 | PASS |
| parakeet-tdt-0.6b-v2 | cpu->cpu | 3.0s | 9.7s | 16.4x | 190 | PASS |
| parakeet-tdt-0.6b-v2 | cuda->cuda | 3.5s | 9.9s | 16.1x | 186 | PASS |
| parakeet-tdt-0.6b-v2_vulkan | vulkan->cpu | 3.0s | 9.6s | 16.6x | 192 | PASS |

## Backend Comparison

**CUDA:** 2.6s load, 5.8s transcribe, 55.6x RTF
**CPU:** 1.9s load, 5.6s transcribe, 60.5x RTF

## Best
**Fastest:** whisper-base_cuda (1.5s)
**Most Efficient:** whisper-base_cuda (106.8x RTF)