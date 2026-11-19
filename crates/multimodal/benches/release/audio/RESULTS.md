# The multimodal release comparison: 1.16.0 against 1.15.0

38 clip cells (2 processor configurations by 19 clip classes), time per clip on one core: 0 faster by more than 2 %, 25 level (within 2 %), 13 slower by more than 2 %. Medians of 8 interleaved rounds; change = the head's time over the base's, minus one, so a negative figure is a faster head.

1.16.0 is `378df2a52` built with cargo 1.100.0 (3acc79c0c 2026-11-05); 1.15.0 is `82c0f1fcd` built with cargo 1.98.0 (797e8a9bc 2026-08-05); both sides run the harness source of `378df2a52`. Measured 2026-11-19. What a cell measures is in `crates/multimodal/benches/release/README.md`; beside this file, `environment.json` holds the identities, `cells.csv` every cell and `rounds.csv` every window.

Cells that read slower by more than 2 % (in how many rounds the head was the slower side, and the head's time over the base's in every round):

- `qwen3_omni` 16-bit 16 kHz mono, 1 s: +4 % (0.59 ms against 0.57 ms; slower in 7 of 8 rounds: 1.062 1.112 1.055 0.977 1.029 1.039 1.002 1.057)
- `qwen3_asr` 16-bit 44.1 kHz mono, 30 s: +3 % (713 ms against 691 ms; slower in 8 of 8 rounds: 1.032 1.031 1.030 1.031 1.033 1.028 1.030 1.034)
- `qwen3_omni` 16-bit 44.1 kHz mono, 30 s: +3 % (712 ms against 691 ms; slower in 8 of 8 rounds: 1.031 1.030 1.032 1.030 1.027 1.031 1.033 1.080)
- `qwen3_asr` 16-bit 22.05 kHz mono, 30 s: +3 % (688 ms against 668 ms; slower in 8 of 8 rounds: 1.030 1.027 1.028 1.029 1.039 1.029 1.036 1.028)
- `qwen3_asr` 16-bit 44.1 kHz stereo, 30 s: +3 % (714 ms against 694 ms; slower in 8 of 8 rounds: 1.028 1.030 1.030 1.032 1.032 1.029 1.033 1.031)
- `qwen3_omni` 16-bit 22.05 kHz mono, 30 s: +3 % (688 ms against 669 ms; slower in 8 of 8 rounds: 1.033 1.025 1.028 1.030 1.000 1.030 1.028 1.031)
- `qwen3_omni` 16-bit 44.1 kHz stereo, 30 s: +3 % (714 ms against 695 ms; slower in 8 of 8 rounds: 1.031 1.017 1.030 1.020 1.026 1.009 1.040 1.102)
- `qwen3_omni` 16-bit 48 kHz stereo, 30 s: +3 % (83.1 ms against 80.9 ms; slower in 7 of 8 rounds: 1.024 1.031 1.022 0.980 1.026 1.036 1.014 1.028)
- `qwen3_omni` 16-bit 48 kHz six channels, 30 s: +3 % (93.7 ms against 91.3 ms; slower in 8 of 8 rounds: 1.030 1.025 1.022 1.018 1.010 1.030 1.032 1.024)
- `qwen3_omni` A-law 8 kHz mono, 30 s: +2 % (37.1 ms against 36.3 ms; slower in 8 of 8 rounds: 1.042 1.022 1.022 1.021 1.017 1.022 1.033 1.023)
- `qwen3_omni` 16-bit 8 kHz mono, 30 s: +2 % (36.8 ms against 36.0 ms; slower in 8 of 8 rounds: 1.005 1.017 1.025 1.038 1.012 1.023 1.024 1.033)
- `qwen3_asr` 16-bit 8 kHz mono, 30 s: +2 % (36.8 ms against 36.0 ms; slower in 8 of 8 rounds: 1.002 1.019 1.022 1.024 1.024 1.017 1.019 1.021)
- `qwen3_asr` A-law 8 kHz mono, 30 s: +2 % (37.0 ms against 36.3 ms; slower in 8 of 8 rounds: 1.017 1.026 1.019 1.020 1.026 1.022 1.018 1.024)

## Every processor configuration, medians over the clip classes

| processor | time per clip | least .. most | CPU per clip | faster / level / slower | same bytes, the standard classes |
|---|---|---|---|---|---|
| `qwen3_asr` | +1 % | -1 % .. +3 % | +1 % | 0 / 14 / 5 | 19 of 19 |
| `qwen3_omni` | +1 % | -1 % .. +4 % | +2 % | 0 / 11 / 8 | 19 of 19 |

## `qwen3_asr`, milliseconds per clip

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per clip | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| 16-bit 16 kHz mono, 0.1 s | 0.15 | +1 % | 0.14 | +1 % | yes |
| 16-bit 16 kHz mono, 1 s | 0.58 | +1 % | 0.57 | +1 % | yes |
| 16-bit 16 kHz mono, 30 s | 15.0 | 0 % | 14.9 | 0 % | yes |
| 16-bit 16 kHz mono, 600 s | 318 | +1 % | 316 | +1 % | yes |
| 16-bit 16 kHz mono, clipped signal, 30 s | 14.8 | -1 % | 14.7 | -1 % | yes |
| 16-bit 16 kHz mono, constant offset, 30 s | 14.9 | 0 % | 14.8 | 0 % | yes |
| 16-bit 16 kHz mono, silence, 30 s | 14.8 | 0 % | 14.7 | 0 % | yes |
| 16-bit 8 kHz mono, 30 s | 36.8 | +2 % | 36.6 | +2 % | yes |
| 16-bit 22.05 kHz mono, 30 s | 688 | +3 % | 684 | +3 % | yes |
| 16-bit 44.1 kHz mono, 30 s | 713 | +3 % | 709 | +3 % | yes |
| 16-bit 48 kHz mono, 30 s | 79.8 | +2 % | 79.3 | +2 % | yes |
| 16-bit 96 kHz mono, 30 s | 144 | 0 % | 143 | 0 % | yes |
| 16-bit 44.1 kHz stereo, 30 s | 714 | +3 % | 710 | +3 % | yes |
| 16-bit 48 kHz stereo, 30 s | 82.7 | +2 % | 82.2 | +2 % | yes |
| 16-bit 48 kHz six channels, 30 s | 93.2 | +1 % | 92.7 | +1 % | yes |
| 24-bit 16 kHz mono, 30 s | 15.1 | 0 % | 15.0 | 0 % | yes |
| 32-bit 16 kHz mono, 30 s | 15.0 | +1 % | 14.9 | +1 % | yes |
| float 16 kHz mono, 30 s | 14.7 | -1 % | 14.6 | -1 % | yes |
| A-law 8 kHz mono, 30 s | 37.0 | +2 % | 36.8 | +2 % | yes |

## `qwen3_omni`, milliseconds per clip

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per clip | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| 16-bit 16 kHz mono, 0.1 s | 0.16 | +1 % | 0.15 | +2 % | yes |
| 16-bit 16 kHz mono, 1 s | 0.59 | +4 % | 0.58 | +4 % | yes |
| 16-bit 16 kHz mono, 30 s | 15.0 | 0 % | 14.9 | 0 % | yes |
| 16-bit 16 kHz mono, 600 s | 319 | +1 % | 317 | +1 % | yes |
| 16-bit 16 kHz mono, clipped signal, 30 s | 14.8 | 0 % | 14.7 | 0 % | yes |
| 16-bit 16 kHz mono, constant offset, 30 s | 14.9 | +1 % | 14.7 | 0 % | yes |
| 16-bit 16 kHz mono, silence, 30 s | 14.8 | 0 % | 14.7 | 0 % | yes |
| 16-bit 8 kHz mono, 30 s | 36.8 | +2 % | 36.6 | +2 % | yes |
| 16-bit 22.05 kHz mono, 30 s | 688 | +3 % | 684 | +3 % | yes |
| 16-bit 44.1 kHz mono, 30 s | 712 | +3 % | 708 | +3 % | yes |
| 16-bit 48 kHz mono, 30 s | 79.8 | +2 % | 79.4 | +2 % | yes |
| 16-bit 96 kHz mono, 30 s | 145 | 0 % | 144 | 0 % | yes |
| 16-bit 44.1 kHz stereo, 30 s | 714 | +3 % | 710 | +3 % | yes |
| 16-bit 48 kHz stereo, 30 s | 83.1 | +3 % | 82.6 | +3 % | yes |
| 16-bit 48 kHz six channels, 30 s | 93.7 | +3 % | 93.2 | +3 % | yes |
| 24-bit 16 kHz mono, 30 s | 15.1 | +1 % | 15.0 | +1 % | yes |
| 32-bit 16 kHz mono, 30 s | 15.0 | +1 % | 14.9 | +1 % | yes |
| float 16 kHz mono, 30 s | 14.8 | -1 % | 14.7 | -1 % | yes |
| A-law 8 kHz mono, 30 s | 37.1 | +2 % | 36.9 | +2 % | yes |
