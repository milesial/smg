# The multimodal release comparison: 1.16.0 against 1.15.0

380 image cells (20 processor configurations by 19 image classes), time per image on one core: 334 faster by more than 2 %, 40 level (within 2 %), 6 slower by more than 2 %. Medians of 15 interleaved rounds; change = the head's time over the base's, minus one, so a negative figure is a faster head.

1.16.0 is `378df2a52` built with cargo 1.100.0 (3acc79c0c 2026-11-05); 1.15.0 is `82c0f1fcd` built with cargo 1.98.0 (797e8a9bc 2026-08-05); both sides run the harness source of `378df2a52`. Measured 2026-11-18. What a cell measures is in `crates/multimodal/benches/release/README.md`; beside this file, `environment.json` holds the identities, `cells.csv` every cell and `rounds.csv` every window.

Cells that read slower by more than 2 % (in how many rounds the head was the slower side, and the head's time over the base's in every round):

- `llava` JPEG 8192x8192: +8 % (330 ms against 304 ms; slower in 15 of 15 rounds: 1.077 1.121 1.116 1.097 1.052 1.062 1.064 1.109 1.075 1.080 1.095 1.102 1.072 1.075 1.089)
- `llava_next` JPEG 8192x8192: +6 % (369 ms against 347 ms; slower in 15 of 15 rounds: 1.073 1.073 1.098 1.079 1.054 1.053 1.051 1.087 1.056 1.054 1.073 1.035 1.070 1.076 1.060)
- `step3_vl` PNG 256x256: +6 % (2.71 ms against 2.56 ms; slower in 11 of 15 rounds: 1.026 1.053 0.890 1.025 1.256 1.228 0.924 0.925 1.042 1.100 1.049 1.083 0.940 1.011 1.241)
- `qwen3_vl` (second config) TIFF: +4 % (2.93 ms against 2.83 ms; slower in 9 of 15 rounds: 1.322 0.999 1.124 1.167 0.931 0.892 1.037 0.878 1.068 0.941 0.924 1.036 1.026 1.112 1.037)
- `step3_vl` JPEG 256x256: +3 % (3.28 ms against 3.20 ms; slower in 8 of 15 rounds: 1.070 0.973 1.045 0.932 0.990 1.053 0.996 1.128 0.982 1.045 0.865 1.109 1.016 0.965 1.008)
- `qwen3_omni` TIFF: +2 % (2.86 ms against 2.81 ms; slower in 9 of 15 rounds: 1.105 0.867 1.187 1.144 0.911 1.189 1.067 0.778 0.956 1.194 1.274 1.051 1.004 0.899 0.899)

## Every processor configuration, medians over the image classes

| processor | time per image | least .. most | CPU per image | batch of ten | faster / level / slower | same bytes, the standard classes | same bytes, the wider list on two wires, alpha composited and kept | the reference's time over the head's (least .. most) | the reference's bytes: decoded pixels, float32 | the base's float32 against the reference |
|---|---|---|---|---|---|---|---|---|---|---|
| `deepseek_v41` | -14 % | -22 % .. -1 % | -14 % | -41 % | 18 / 1 / 0 | 20 of 20 | 236 of 236 | - (no such class in the library) | - | - |
| `gemma4` | -46 % | -74 % .. -12 % | -46 % | -67 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | 1.90x (1.20x .. 4.34x) | 20 of 20, 20 of 20 | 20 of 20 |
| `glm53_flash` | -49 % | -63 % .. -24 % | -49 % | -41 % | 19 / 0 / 0 | 20 of 20 | 224 of 224 (12 refused on both) | 3.10x (1.58x .. 4.50x) | 20 of 20, 20 of 20 | 20 of 20 |
| `kimi_k25` | -34 % | -44 % .. -8 % | -34 % | -59 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | 2.38x (1.36x .. 3.75x) | 20 of 20, 0 of 20 | 0 of 20 |
| `kimi_k3` | -36 % | -60 % .. -11 % | -36 % | -59 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | - (its processor is checkpoint code) | - | - |
| `llama4` | -3 % | -19 % .. +1 % | -3 % | +1 % | 10 / 9 / 0 | 0 of 20 | 0 of 236 | 0.81x (0.55x .. 4.38x) | 20 of 20, 0 of 20 | 0 of 20 |
| `llava_next` | -17 % | -37 % .. +6 % | -16 % | -16 % | 18 / 0 / 1 | 0 of 20 | 0 of 236 | 1.78x (1.26x .. 3.34x) | 20 of 20, 20 of 20 | 0 of 20 |
| `llava` | -17 % | -31 % .. +8 % | -17 % | -9 % | 18 / 0 / 1 | 0 of 20 | 0 of 236 | 1.95x (1.11x .. 3.56x) | 20 of 20, 20 of 20 | 0 of 20 |
| `llava` with the `pixtral` processor | -28 % | -40 % .. -8 % | -28 % | -26 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | 3.80x (1.55x .. 5.59x) | 20 of 20, 20 of 20 | 20 of 20 |
| `minimax_m3` | -34 % | -46 % .. -11 % | -34 % | -45 % | 19 / 0 / 0 | 20 of 20 | 224 of 224 (12 refused on both) | 2.27x (1.41x .. 3.97x) | 20 of 20, 20 of 20 | 20 of 20 |
| `muse_spark` | -29 % | -32 % .. -15 % | -29 % | -24 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | - (no such class in the library) | - | - |
| `nemotron_omni` | -33 % | -48 % .. -4 % | -33 % | -40 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | - (its processor is checkpoint code) | - | - |
| `phi3_v` | -47 % | -58 % .. -30 % | -48 % | -44 % | 19 / 0 / 0 | 20 of 20 | 236 of 236 | - (its processor is checkpoint code) | - | - |
| `qwen3_omni` | -11 % | -30 % .. +2 % | -10 % | -1 % | 11 / 7 / 1 | 20 of 20 | 224 of 224 (12 refused on both) | 3.81x (1.74x .. 5.76x) | 20 of 20, 20 of 20 | 20 of 20 |
| `qwen3_vl` | -10 % | -29 % .. 0 % | -10 % | -2 % | 12 / 7 / 0 | 20 of 20 | 224 of 224 (12 refused on both) | 3.79x (1.74x .. 5.02x) | 20 of 20, 20 of 20 | 20 of 20 |
| `qwen3_vl` (second config) | -12 % | -30 % .. +4 % | -12 % | -2 % | 12 / 6 / 1 | 20 of 20 | 224 of 224 (12 refused on both) | 3.79x (1.77x .. 5.15x) | 20 of 20, 20 of 20 | 20 of 20 |
| `qwen_vl` | -40 % | -50 % .. -21 % | -40 % | -37 % | 19 / 0 / 0 | 20 of 20 | 224 of 224 (12 refused on both) | 3.11x (1.69x .. 4.15x) | 20 of 20, 20 of 20 | 20 of 20 |
| `qwen_vl` (second config) | -40 % | -52 % .. -21 % | -40 % | -37 % | 19 / 0 / 0 | 20 of 20 | 224 of 224 (12 refused on both) | 3.09x (1.67x .. 4.24x) | 20 of 20, 20 of 20 | 20 of 20 |
| `step3_vl` | -2 % | -13 % .. +6 % | -3 % | -1 % | 11 / 6 / 2 | 20 of 20 | 228 of 228 (8 refused on both) | - (no such class in the library) | - | - |
| `step3_vl` (model type `step5_vl`) | -3 % | -14 % .. +1 % | -3 % | 0 % | 15 / 4 / 0 | 20 of 20 | 228 of 228 (8 refused on both) | - (no such class in the library) | - | - |

## `deepseek_v41`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 1.42 | -15 % | 1.40 | -15 % | yes |
| JPEG 1024x768 | 5.72 | -14 % | 5.67 | -14 % | yes |
| JPEG 1920x1080 | 14.3 | -13 % | 14.2 | -13 % | yes |
| JPEG 4096x2304 | 39.7 | -4 % | 39.5 | -4 % | yes |
| JPEG 8192x8192 | 305 | -1 % | 303 | -1 % | yes |
| JPEG progressive | 8.65 | -8 % | 8.58 | -8 % | yes |
| JPEG CMYK | 10.2 | -6 % | 10.1 | -6 % | yes |
| JPEG greyscale | 5.20 | -13 % | 5.15 | -13 % | yes |
| JPEG EXIF-rotated | 2.82 | -20 % | 2.79 | -20 % | yes |
| PNG 256x256 | 1.41 | -13 % | 1.39 | -12 % | yes |
| PNG 1024x768 | 7.80 | -15 % | 7.74 | -15 % | yes |
| PNG 1920x1080 | 19.3 | -17 % | 19.2 | -17 % | yes |
| PNG 3840x2160 | 59.4 | -14 % | 59.0 | -14 % | yes |
| PNG RGBA (alpha) | 10.2 | -22 % | 10.1 | -22 % | yes |
| WebP lossy | 7.17 | -9 % | 7.11 | -9 % | yes |
| WebP lossless | 17.9 | -4 % | 17.7 | -4 % | yes |
| GIF | 10.0 | -17 % | 9.96 | -17 % | yes |
| BMP | 5.67 | -14 % | 5.62 | -14 % | yes |
| TIFF | 4.84 | -16 % | 4.79 | -16 % | yes |
| batch of ten 1024x768 JPEGs | 5.36 | -41 % | 5.33 | -41 % | yes |

## `gemma4`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 1.51 | -74 % | 1.49 | -74 % | yes | 6.56 | 4.34x | yes |
| JPEG 1024x768 | 4.53 | -58 % | 4.50 | -58 % | yes | 10.6 | 2.33x | yes |
| JPEG 1920x1080 | 9.60 | -46 % | 9.54 | -46 % | yes | 18.2 | 1.90x | yes |
| JPEG 4096x2304 | 34.1 | -31 % | 33.9 | -31 % | yes | 54.7 | 1.61x | yes |
| JPEG 8192x8192 | 303 | -12 % | 301 | -12 % | yes | 439 | 1.45x | yes |
| JPEG progressive | 7.73 | -44 % | 7.67 | -44 % | yes | 12.7 | 1.65x | yes |
| JPEG CMYK | 9.17 | -40 % | 9.10 | -40 % | yes | 15.9 | 1.73x | yes |
| JPEG greyscale | 4.20 | -59 % | 4.13 | -60 % | yes | 8.17 | 1.94x | yes |
| JPEG EXIF-rotated | 2.99 | -66 % | 2.96 | -66 % | yes | 6.83 | 2.29x | yes |
| PNG 256x256 | 1.51 | -72 % | 1.49 | -72 % | yes | 6.51 | 4.32x | yes |
| PNG 1024x768 | 6.78 | -49 % | 6.72 | -49 % | yes | 19.5 | 2.88x | yes |
| PNG 1920x1080 | 14.9 | -41 % | 14.8 | -41 % | yes | 41.6 | 2.80x | yes |
| PNG 3840x2160 | 53.8 | -29 % | 53.5 | -29 % | yes | 147 | 2.74x | yes |
| PNG RGBA (alpha) | 9.23 | -46 % | 9.16 | -46 % | yes | 25.1 | 2.71x | yes |
| WebP lossy | 6.23 | -49 % | 6.18 | -50 % | yes | 11.4 | 1.83x | yes |
| WebP lossless | 16.9 | -27 % | 16.8 | -27 % | yes | 21.9 | 1.29x | yes |
| GIF | 9.15 | -44 % | 9.08 | -44 % | yes | 11.0 | 1.20x | yes |
| BMP | 4.52 | -58 % | 4.48 | -58 % | yes | 7.50 | 1.66x | yes |
| TIFF | 3.79 | -61 % | 3.76 | -61 % | yes | 7.01 | 1.85x | yes |
| batch of ten 1024x768 JPEGs | 4.36 | -67 % | 4.33 | -67 % | yes | 11.5 | 2.63x | yes |

## `glm53_flash`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.60 | -53 % | 0.59 | -53 % | yes | 2.22 | 3.70x | yes |
| JPEG 1024x768 | 4.20 | -54 % | 4.16 | -54 % | yes | 13.0 | 3.10x | yes |
| JPEG 1920x1080 | 11.4 | -55 % | 11.3 | -55 % | yes | 51.4 | 4.50x | yes |
| JPEG 4096x2304 | 93.1 | -49 % | 92.1 | -49 % | yes | 213 | 2.29x | yes |
| JPEG 8192x8192 | 375 | -28 % | 373 | -28 % | yes | 703 | 1.88x | yes |
| JPEG progressive | 7.19 | -42 % | 7.14 | -42 % | yes | 17.2 | 2.39x | yes |
| JPEG CMYK | 8.58 | -39 % | 8.52 | -39 % | yes | 19.9 | 2.32x | yes |
| JPEG greyscale | 3.54 | -61 % | 3.51 | -61 % | yes | 13.5 | 3.81x | yes |
| JPEG EXIF-rotated | 2.17 | -54 % | 2.14 | -54 % | yes | 5.18 | 2.39x | yes |
| PNG 256x256 | 0.67 | -51 % | 0.65 | -51 % | yes | 2.80 | 4.21x | yes |
| PNG 1024x768 | 6.22 | -49 % | 6.17 | -49 % | yes | 23.5 | 3.78x | yes |
| PNG 1920x1080 | 16.5 | -50 % | 16.4 | -50 % | yes | 72.0 | 4.37x | yes |
| PNG 3840x2160 | 112 | -46 % | 111 | -46 % | yes | 298 | 2.66x | yes |
| PNG RGBA (alpha) | 8.68 | -46 % | 8.61 | -46 % | yes | 29.6 | 3.41x | yes |
| WebP lossy | 5.63 | -48 % | 5.58 | -48 % | yes | 15.5 | 2.76x | yes |
| WebP lossless | 16.4 | -24 % | 16.3 | -24 % | yes | 25.9 | 1.58x | yes |
| GIF | 8.42 | -45 % | 8.35 | -45 % | yes | 14.8 | 1.76x | yes |
| BMP | 4.04 | -57 % | 4.01 | -58 % | yes | 12.6 | 3.12x | yes |
| TIFF | 3.24 | -63 % | 3.21 | -63 % | yes | 13.5 | 4.16x | yes |
| batch of ten 1024x768 JPEGs | 8.39 | -41 % | 8.34 | -41 % | yes | 19.0 | 2.26x | yes |

## `kimi_k25`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.57 | -34 % | 0.55 | -34 % | yes | 1.14 | 2.01x | pixels yes, float32 no |
| JPEG 1024x768 | 3.90 | -39 % | 3.87 | -39 % | yes | 10.6 | 2.72x | pixels yes, float32 no |
| JPEG 1920x1080 | 10.4 | -44 % | 10.4 | -44 % | yes | 25.9 | 2.49x | pixels yes, float32 no |
| JPEG 4096x2304 | 44.9 | -35 % | 44.6 | -35 % | yes | 119 | 2.65x | pixels yes, float32 no |
| JPEG 8192x8192 | 312 | -8 % | 309 | -8 % | yes | 549 | 1.76x | pixels yes, float32 no |
| JPEG progressive | 7.00 | -22 % | 6.95 | -22 % | yes | 13.7 | 1.96x | pixels yes, float32 no |
| JPEG CMYK | 8.44 | -20 % | 8.38 | -20 % | yes | 16.1 | 1.91x | pixels yes, float32 no |
| JPEG greyscale | 3.55 | -37 % | 3.52 | -37 % | yes | 8.45 | 2.38x | pixels yes, float32 no |
| JPEG EXIF-rotated | 2.21 | -33 % | 2.19 | -33 % | yes | 4.84 | 2.19x | pixels yes, float32 no |
| PNG 256x256 | 0.66 | -29 % | 0.64 | -29 % | yes | 2.47 | 3.75x | pixels yes, float32 no |
| PNG 1024x768 | 6.05 | -34 % | 5.97 | -34 % | yes | 19.2 | 3.18x | pixels yes, float32 no |
| PNG 1920x1080 | 15.7 | -39 % | 15.6 | -39 % | yes | 50.5 | 3.21x | pixels yes, float32 no |
| PNG 3840x2160 | 63.5 | -34 % | 63.0 | -34 % | yes | 214 | 3.37x | pixels yes, float32 no |
| PNG RGBA (alpha) | 8.54 | -31 % | 8.47 | -31 % | yes | 26.2 | 3.07x | pixels yes, float32 no |
| WebP lossy | 5.51 | -25 % | 5.47 | -25 % | yes | 11.8 | 2.14x | pixels yes, float32 no |
| WebP lossless | 16.3 | -10 % | 16.2 | -10 % | yes | 22.1 | 1.36x | pixels yes, float32 no |
| GIF | 8.32 | -29 % | 8.26 | -29 % | yes | 11.4 | 1.37x | pixels yes, float32 no |
| BMP | 3.89 | -37 % | 3.85 | -37 % | yes | 7.91 | 2.03x | pixels yes, float32 no |
| TIFF | 3.18 | -40 % | 3.15 | -41 % | yes | 7.84 | 2.47x | pixels yes, float32 no |
| batch of ten 1024x768 JPEGs | 3.82 | -59 % | 3.79 | -59 % | yes | 12.2 | 3.20x | pixels yes, float32 no |

## `kimi_k3`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 0.57 | -36 % | 0.56 | -36 % | yes |
| JPEG 1024x768 | 3.92 | -38 % | 3.89 | -38 % | yes |
| JPEG 1920x1080 | 10.4 | -44 % | 10.4 | -44 % | yes |
| JPEG 4096x2304 | 45.9 | -60 % | 45.6 | -60 % | yes |
| JPEG 8192x8192 | 398 | -21 % | 395 | -21 % | yes |
| JPEG progressive | 6.99 | -23 % | 6.93 | -23 % | yes |
| JPEG CMYK | 8.44 | -19 % | 8.37 | -19 % | yes |
| JPEG greyscale | 3.50 | -36 % | 3.47 | -36 % | yes |
| JPEG EXIF-rotated | 2.17 | -34 % | 2.15 | -34 % | yes |
| PNG 256x256 | 0.65 | -30 % | 0.64 | -30 % | yes |
| PNG 1024x768 | 6.00 | -37 % | 5.95 | -37 % | yes |
| PNG 1920x1080 | 15.7 | -40 % | 15.6 | -40 % | yes |
| PNG 3840x2160 | 62.0 | -52 % | 61.5 | -52 % | yes |
| PNG RGBA (alpha) | 8.47 | -32 % | 8.39 | -32 % | yes |
| WebP lossy | 5.51 | -27 % | 5.47 | -27 % | yes |
| WebP lossless | 16.3 | -11 % | 16.2 | -11 % | yes |
| GIF | 8.29 | -28 % | 8.23 | -28 % | yes |
| BMP | 3.81 | -37 % | 3.78 | -37 % | yes |
| TIFF | 3.06 | -43 % | 3.02 | -43 % | yes |
| batch of ten 1024x768 JPEGs | 3.83 | -59 % | 3.80 | -59 % | yes |

## `llama4`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 1.27 | -19 % | 1.26 | -19 % | no | 4.82 | 3.78x | pixels yes, float32 no |
| JPEG 1024x768 | 24.2 | -2 % | 24.0 | -2 % | no | 15.8 | 0.66x | pixels yes, float32 no |
| JPEG 1920x1080 | 38.3 | -3 % | 38.0 | -3 % | no | 30.9 | 0.81x | pixels yes, float32 no |
| JPEG 4096x2304 | 64.4 | -1 % | 63.9 | -1 % | no | 77.3 | 1.20x | pixels yes, float32 no |
| JPEG 8192x8192 | 331 | -1 % | 329 | -1 % | no | 503 | 1.52x | pixels yes, float32 no |
| JPEG progressive | 27.3 | -1 % | 27.0 | -1 % | no | 19.4 | 0.71x | pixels yes, float32 no |
| JPEG CMYK | 28.7 | -1 % | 28.5 | -1 % | no | 22.2 | 0.77x | pixels yes, float32 no |
| JPEG greyscale | 23.6 | -3 % | 23.4 | -3 % | no | 14.3 | 0.61x | pixels yes, float32 no |
| JPEG EXIF-rotated | 9.61 | -8 % | 9.53 | -8 % | no | 9.25 | 0.96x | pixels yes, float32 no |
| PNG 256x256 | 1.33 | -17 % | 1.31 | -17 % | no | 5.83 | 4.38x | pixels yes, float32 no |
| PNG 1024x768 | 26.4 | -3 % | 26.2 | -3 % | no | 25.7 | 0.97x | pixels yes, float32 no |
| PNG 1920x1080 | 43.2 | -8 % | 42.9 | -8 % | no | 54.1 | 1.25x | pixels yes, float32 no |
| PNG 3840x2160 | 83.0 | -9 % | 82.4 | -10 % | no | 161 | 1.94x | pixels yes, float32 no |
| PNG RGBA (alpha) | 28.6 | -8 % | 28.4 | -8 % | no | 32.2 | 1.12x | pixels yes, float32 no |
| WebP lossy | 25.8 | 0 % | 25.6 | 0 % | no | 17.8 | 0.69x | pixels yes, float32 no |
| WebP lossless | 36.9 | +1 % | 36.6 | +1 % | no | 28.3 | 0.77x | pixels yes, float32 no |
| GIF | 28.5 | -5 % | 28.3 | -5 % | no | 17.2 | 0.60x | pixels yes, float32 no |
| BMP | 24.1 | -2 % | 23.9 | -2 % | no | 13.2 | 0.55x | pixels yes, float32 no |
| TIFF | 23.6 | +1 % | 23.4 | +1 % | no | 13.2 | 0.56x | pixels yes, float32 no |
| batch of ten 1024x768 JPEGs | 35.4 | +1 % | 35.1 | +1 % | no | 18.4 | 0.52x | pixels yes, float32 no |

## `llava_next`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 1.73 | -37 % | 1.71 | -37 % | no | 5.15 | 2.98x | yes |
| JPEG 1024x768 | 6.23 | -15 % | 6.18 | -15 % | no | 11.9 | 1.91x | yes |
| JPEG 1920x1080 | 11.4 | -9 % | 11.3 | -9 % | no | 20.3 | 1.78x | yes |
| JPEG 4096x2304 | 40.2 | -6 % | 40.0 | -6 % | no | 66.8 | 1.66x | yes |
| JPEG 8192x8192 | 369 | +6 % | 367 | +6 % | no | 488 | 1.32x | yes |
| JPEG progressive | 9.11 | -11 % | 9.04 | -11 % | no | 14.9 | 1.64x | yes |
| JPEG CMYK | 10.4 | -10 % | 10.4 | -10 % | no | 18.4 | 1.77x | yes |
| JPEG greyscale | 5.47 | -18 % | 5.43 | -18 % | no | 10.3 | 1.88x | yes |
| JPEG EXIF-rotated | 4.24 | -31 % | 4.20 | -31 % | no | 9.56 | 2.26x | yes |
| PNG 256x256 | 1.64 | -32 % | 1.62 | -33 % | no | 5.48 | 3.34x | yes |
| PNG 1024x768 | 7.98 | -20 % | 7.92 | -20 % | no | 20.7 | 2.60x | yes |
| PNG 1920x1080 | 16.6 | -17 % | 16.5 | -16 % | no | 43.8 | 2.64x | yes |
| PNG 3840x2160 | 59.1 | -13 % | 58.7 | -13 % | no | 154 | 2.60x | yes |
| PNG RGBA (alpha) | 10.5 | -23 % | 10.5 | -23 % | no | 27.5 | 2.62x | yes |
| WebP lossy | 7.48 | -13 % | 7.42 | -13 % | no | 12.9 | 1.72x | yes |
| WebP lossless | 18.3 | -6 % | 18.1 | -6 % | no | 24.6 | 1.35x | yes |
| GIF | 10.4 | -19 % | 10.3 | -19 % | no | 13.1 | 1.26x | yes |
| BMP | 6.00 | -17 % | 5.96 | -17 % | no | 9.36 | 1.56x | yes |
| TIFF | 5.26 | -18 % | 5.22 | -18 % | no | 9.08 | 1.73x | yes |
| batch of ten 1024x768 JPEGs | 7.82 | -16 % | 7.77 | -16 % | no | 14.3 | 1.83x | yes |

## `llava`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.99 | -28 % | 0.98 | -28 % | no | 2.48 | 2.49x | yes |
| JPEG 1024x768 | 3.53 | -16 % | 3.49 | -16 % | no | 6.71 | 1.90x | yes |
| JPEG 1920x1080 | 8.26 | -10 % | 8.21 | -10 % | no | 16.7 | 2.02x | yes |
| JPEG 4096x2304 | 30.7 | -11 % | 30.5 | -11 % | no | 62.8 | 2.05x | yes |
| JPEG 8192x8192 | 330 | +8 % | 328 | +9 % | no | 437 | 1.32x | yes |
| JPEG progressive | 6.65 | -10 % | 6.59 | -10 % | no | 9.91 | 1.49x | yes |
| JPEG CMYK | 8.08 | -7 % | 8.02 | -7 % | no | 13.7 | 1.69x | yes |
| JPEG greyscale | 3.19 | -15 % | 3.16 | -15 % | no | 6.42 | 2.01x | yes |
| JPEG EXIF-rotated | 2.39 | -31 % | 2.35 | -32 % | no | 4.67 | 1.95x | yes |
| PNG 256x256 | 0.98 | -30 % | 0.96 | -30 % | no | 3.49 | 3.56x | yes |
| PNG 1024x768 | 5.68 | -19 % | 5.63 | -19 % | no | 16.9 | 2.98x | yes |
| PNG 1920x1080 | 13.5 | -18 % | 13.4 | -18 % | no | 39.4 | 2.91x | yes |
| PNG 3840x2160 | 50.8 | -17 % | 50.4 | -17 % | no | 153 | 3.00x | yes |
| PNG RGBA (alpha) | 8.12 | -26 % | 8.05 | -26 % | no | 23.1 | 2.85x | yes |
| WebP lossy | 5.11 | -11 % | 5.06 | -11 % | no | 8.88 | 1.74x | yes |
| WebP lossless | 16.0 | -3 % | 15.9 | -3 % | no | 19.7 | 1.23x | yes |
| GIF | 7.99 | -21 % | 7.92 | -21 % | no | 8.88 | 1.11x | yes |
| BMP | 3.63 | -18 % | 3.59 | -18 % | no | 5.75 | 1.59x | yes |
| TIFF | 2.87 | -20 % | 2.85 | -20 % | no | 5.32 | 1.85x | yes |
| batch of ten 1024x768 JPEGs | 3.59 | -9 % | 3.56 | -9 % | no | 6.13 | 1.71x | yes |

## `llava` with the `pixtral` processor, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.47 | -27 % | 0.46 | -28 % | yes | 1.88 | 4.03x | yes |
| JPEG 1024x768 | 3.06 | -28 % | 3.03 | -28 % | yes | 13.6 | 4.46x | yes |
| JPEG 1920x1080 | 9.47 | -40 % | 9.41 | -40 % | yes | 22.2 | 2.35x | yes |
| JPEG 4096x2304 | 33.9 | -28 % | 33.7 | -28 % | yes | 68.7 | 2.02x | yes |
| JPEG 8192x8192 | 297 | -16 % | 295 | -16 % | yes | 461 | 1.55x | yes |
| JPEG progressive | 6.12 | -19 % | 6.07 | -19 % | yes | 20.0 | 3.27x | yes |
| JPEG CMYK | 7.58 | -16 % | 7.52 | -16 % | yes | 19.1 | 2.52x | yes |
| JPEG greyscale | 2.61 | -34 % | 2.58 | -34 % | yes | 12.2 | 4.69x | yes |
| JPEG EXIF-rotated | 1.78 | -35 % | 1.76 | -35 % | yes | 7.14 | 4.02x | yes |
| PNG 256x256 | 0.55 | -27 % | 0.54 | -27 % | yes | 2.92 | 5.31x | yes |
| PNG 1024x768 | 5.16 | -29 % | 5.09 | -29 % | yes | 23.6 | 4.57x | yes |
| PNG 1920x1080 | 14.8 | -36 % | 14.7 | -36 % | yes | 45.6 | 3.07x | yes |
| PNG 3840x2160 | 53.7 | -28 % | 53.4 | -28 % | yes | 151 | 2.81x | yes |
| PNG RGBA (alpha) | 7.63 | -30 % | 7.56 | -30 % | yes | 29.4 | 3.86x | yes |
| WebP lossy | 4.61 | -23 % | 4.57 | -23 % | yes | 19.6 | 4.25x | yes |
| WebP lossless | 15.4 | -8 % | 15.3 | -8 % | yes | 26.0 | 1.68x | yes |
| GIF | 7.26 | -29 % | 7.20 | -29 % | yes | 15.5 | 2.13x | yes |
| BMP | 2.92 | -36 % | 2.90 | -36 % | yes | 11.1 | 3.80x | yes |
| TIFF | 2.12 | -36 % | 2.10 | -36 % | yes | 11.8 | 5.59x | yes |
| batch of ten 1024x768 JPEGs | 3.33 | -26 % | 3.31 | -26 % | yes | 17.0 | 5.10x | yes |

## `minimax_m3`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.75 | -44 % | 0.74 | -44 % | yes | 2.10 | 2.78x | yes |
| JPEG 1024x768 | 4.89 | -41 % | 4.86 | -41 % | yes | 12.6 | 2.58x | yes |
| JPEG 1920x1080 | 10.2 | -33 % | 10.1 | -33 % | yes | 19.9 | 1.95x | yes |
| JPEG 4096x2304 | 34.6 | -25 % | 34.4 | -25 % | yes | 61.1 | 1.76x | yes |
| JPEG 8192x8192 | 303 | -11 % | 301 | -11 % | yes | 431 | 1.42x | yes |
| JPEG progressive | 8.00 | -29 % | 7.94 | -29 % | yes | 14.8 | 1.86x | yes |
| JPEG CMYK | 9.43 | -25 % | 9.36 | -25 % | yes | 18.1 | 1.92x | yes |
| JPEG greyscale | 4.47 | -42 % | 4.43 | -43 % | yes | 10.9 | 2.44x | yes |
| JPEG EXIF-rotated | 3.04 | -42 % | 3.01 | -42 % | yes | 7.92 | 2.60x | yes |
| PNG 256x256 | 0.80 | -46 % | 0.78 | -46 % | yes | 3.17 | 3.97x | yes |
| PNG 1024x768 | 7.04 | -37 % | 6.98 | -37 % | yes | 21.5 | 3.06x | yes |
| PNG 1920x1080 | 15.6 | -31 % | 15.5 | -31 % | yes | 44.4 | 2.85x | yes |
| PNG 3840x2160 | 54.5 | -25 % | 54.1 | -25 % | yes | 148 | 2.72x | yes |
| PNG RGBA (alpha) | 9.57 | -36 % | 9.48 | -36 % | yes | 27.2 | 2.84x | yes |
| WebP lossy | 6.42 | -34 % | 6.37 | -34 % | yes | 13.4 | 2.08x | yes |
| WebP lossless | 17.2 | -16 % | 17.1 | -16 % | yes | 24.3 | 1.41x | yes |
| GIF | 9.35 | -34 % | 9.28 | -34 % | yes | 13.4 | 1.43x | yes |
| BMP | 4.84 | -43 % | 4.80 | -43 % | yes | 9.98 | 2.06x | yes |
| TIFF | 4.26 | -43 % | 4.22 | -43 % | yes | 9.69 | 2.27x | yes |
| batch of ten 1024x768 JPEGs | 4.42 | -45 % | 4.39 | -45 % | yes | 15.4 | 3.48x | yes |

## `muse_spark`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 1.28 | -29 % | 1.25 | -30 % | yes |
| JPEG 1024x768 | 9.68 | -28 % | 9.61 | -28 % | yes |
| JPEG 1920x1080 | 26.2 | -28 % | 26.0 | -28 % | yes |
| JPEG 4096x2304 | 63.1 | -30 % | 62.7 | -30 % | yes |
| JPEG 8192x8192 | 341 | -20 % | 339 | -20 % | yes |
| JPEG progressive | 12.7 | -24 % | 12.6 | -24 % | yes |
| JPEG CMYK | 14.3 | -21 % | 14.2 | -21 % | yes |
| JPEG greyscale | 9.28 | -30 % | 9.20 | -30 % | yes |
| JPEG EXIF-rotated | 4.63 | -30 % | 4.59 | -30 % | yes |
| PNG 256x256 | 1.32 | -30 % | 1.31 | -31 % | yes |
| PNG 1024x768 | 11.7 | -29 % | 11.6 | -29 % | yes |
| PNG 1920x1080 | 31.5 | -29 % | 31.3 | -29 % | yes |
| PNG 3840x2160 | 82.7 | -28 % | 82.1 | -28 % | yes |
| PNG RGBA (alpha) | 14.3 | -29 % | 14.2 | -29 % | yes |
| WebP lossy | 11.2 | -26 % | 11.2 | -26 % | yes |
| WebP lossless | 21.9 | -15 % | 21.8 | -15 % | yes |
| GIF | 14.2 | -27 % | 14.0 | -27 % | yes |
| BMP | 9.65 | -31 % | 9.54 | -31 % | yes |
| TIFF | 8.86 | -32 % | 8.79 | -32 % | yes |
| batch of ten 1024x768 JPEGs | 12.6 | -24 % | 12.5 | -24 % | yes |

## `nemotron_omni`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 2.49 | -44 % | 2.47 | -44 % | yes |
| JPEG 1024x768 | 11.9 | -33 % | 11.8 | -33 % | yes |
| JPEG 1920x1080 | 32.1 | -33 % | 31.8 | -34 % | yes |
| JPEG 4096x2304 | 146 | -17 % | 145 | -17 % | yes |
| JPEG 8192x8192 | 765 | -4 % | 761 | -4 % | yes |
| JPEG progressive | 14.2 | -31 % | 14.1 | -31 % | yes |
| JPEG CMYK | 15.6 | -29 % | 15.5 | -29 % | yes |
| JPEG greyscale | 10.7 | -37 % | 10.6 | -37 % | yes |
| JPEG EXIF-rotated | 4.75 | -38 % | 4.70 | -38 % | yes |
| PNG 256x256 | 2.28 | -48 % | 2.26 | -48 % | yes |
| PNG 1024x768 | 14.0 | -33 % | 13.9 | -33 % | yes |
| PNG 1920x1080 | 37.3 | -33 % | 37.0 | -33 % | yes |
| PNG 3840x2160 | 159 | -19 % | 158 | -19 % | yes |
| PNG RGBA (alpha) | 15.8 | -34 % | 15.6 | -34 % | yes |
| WebP lossy | 12.7 | -33 % | 12.6 | -33 % | yes |
| WebP lossless | 23.5 | -21 % | 23.3 | -21 % | yes |
| GIF | 15.5 | -33 % | 15.4 | -33 % | yes |
| BMP | 11.1 | -36 % | 11.0 | -36 % | yes |
| TIFF | 10.4 | -38 % | 10.3 | -38 % | yes |
| batch of ten 1024x768 JPEGs | 13.5 | -40 % | 13.4 | -40 % | yes |

## `phi3_v`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 5.37 | -47 % | 5.31 | -48 % | yes |
| JPEG 1024x768 | 7.48 | -53 % | 7.43 | -53 % | yes |
| JPEG 1920x1080 | 12.2 | -41 % | 12.2 | -41 % | yes |
| JPEG 4096x2304 | 35.0 | -32 % | 34.8 | -32 % | yes |
| JPEG 8192x8192 | 290 | -53 % | 288 | -53 % | yes |
| JPEG progressive | 10.6 | -45 % | 10.5 | -45 % | yes |
| JPEG CMYK | 12.1 | -41 % | 12.0 | -41 % | yes |
| JPEG greyscale | 7.00 | -56 % | 6.92 | -56 % | yes |
| JPEG EXIF-rotated | 8.07 | -58 % | 8.02 | -58 % | yes |
| PNG 256x256 | 4.84 | -51 % | 4.80 | -51 % | yes |
| PNG 1024x768 | 9.51 | -50 % | 9.44 | -50 % | yes |
| PNG 1920x1080 | 17.5 | -38 % | 17.4 | -38 % | yes |
| PNG 3840x2160 | 54.5 | -31 % | 54.2 | -31 % | yes |
| PNG RGBA (alpha) | 12.0 | -47 % | 11.9 | -47 % | yes |
| WebP lossy | 9.03 | -49 % | 8.96 | -49 % | yes |
| WebP lossless | 19.7 | -30 % | 19.6 | -30 % | yes |
| GIF | 11.9 | -46 % | 11.8 | -46 % | yes |
| BMP | 7.24 | -56 % | 7.18 | -56 % | yes |
| TIFF | 6.65 | -57 % | 6.58 | -57 % | yes |
| batch of ten 1024x768 JPEGs | 8.96 | -44 % | 8.89 | -44 % | yes |

## `qwen3_omni`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.49 | -30 % | 0.48 | -30 % | yes | 1.95 | 3.98x | yes |
| JPEG 1024x768 | 3.68 | +1 % | 3.63 | +1 % | yes | 18.0 | 4.88x | yes |
| JPEG 1920x1080 | 11.1 | -17 % | 11.0 | -17 % | yes | 63.7 | 5.76x | yes |
| JPEG 4096x2304 | 75.8 | 0 % | 75.3 | 0 % | yes | 308 | 4.07x | yes |
| JPEG 8192x8192 | 423 | -23 % | 420 | -23 % | yes | 858 | 2.03x | yes |
| JPEG progressive | 6.75 | -1 % | 6.69 | -1 % | yes | 18.1 | 2.68x | yes |
| JPEG CMYK | 8.15 | -1 % | 8.09 | -1 % | yes | 21.7 | 2.66x | yes |
| JPEG greyscale | 3.22 | -2 % | 3.19 | -2 % | yes | 14.3 | 4.46x | yes |
| JPEG EXIF-rotated | 1.96 | -21 % | 1.95 | -21 % | yes | 6.72 | 3.42x | yes |
| PNG 256x256 | 0.60 | -27 % | 0.59 | -27 % | yes | 2.86 | 4.77x | yes |
| PNG 1024x768 | 5.84 | -11 % | 5.80 | -10 % | yes | 24.8 | 4.25x | yes |
| PNG 1920x1080 | 16.2 | -21 % | 16.1 | -21 % | yes | 79.2 | 4.89x | yes |
| PNG 3840x2160 | 97.2 | -15 % | 96.5 | -15 % | yes | 367 | 3.78x | yes |
| PNG RGBA (alpha) | 8.33 | -20 % | 8.27 | -20 % | yes | 31.7 | 3.81x | yes |
| WebP lossy | 5.27 | 0 % | 5.22 | 0 % | yes | 16.8 | 3.18x | yes |
| WebP lossless | 16.1 | 0 % | 16.0 | 0 % | yes | 28.0 | 1.74x | yes |
| GIF | 8.17 | -13 % | 8.11 | -13 % | yes | 19.3 | 2.36x | yes |
| BMP | 3.62 | -6 % | 3.58 | -6 % | yes | 12.8 | 3.55x | yes |
| TIFF | 2.86 | +2 % | 2.84 | +2 % | yes | 12.8 | 4.46x | yes |
| batch of ten 1024x768 JPEGs | 6.41 | -1 % | 6.37 | -1 % | yes | 21.7 | 3.38x | yes |

## `qwen3_vl`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.50 | -29 % | 0.48 | -30 % | yes | 1.78 | 3.60x | yes |
| JPEG 1024x768 | 3.62 | -1 % | 3.59 | -1 % | yes | 15.0 | 4.14x | yes |
| JPEG 1920x1080 | 10.9 | -18 % | 10.8 | -18 % | yes | 53.9 | 4.94x | yes |
| JPEG 4096x2304 | 73.7 | -1 % | 73.2 | -1 % | yes | 294 | 3.98x | yes |
| JPEG 8192x8192 | 440 | -26 % | 438 | -26 % | yes | 1009 | 2.29x | yes |
| JPEG progressive | 6.75 | 0 % | 6.70 | 0 % | yes | 18.5 | 2.74x | yes |
| JPEG CMYK | 8.14 | -1 % | 8.08 | -1 % | yes | 21.6 | 2.65x | yes |
| JPEG greyscale | 3.21 | -3 % | 3.18 | -3 % | yes | 14.3 | 4.46x | yes |
| JPEG EXIF-rotated | 1.98 | -22 % | 1.96 | -22 % | yes | 6.84 | 3.46x | yes |
| PNG 256x256 | 0.60 | -27 % | 0.59 | -27 % | yes | 3.03 | 5.02x | yes |
| PNG 1024x768 | 5.82 | -10 % | 5.78 | -10 % | yes | 24.3 | 4.17x | yes |
| PNG 1920x1080 | 16.1 | -22 % | 16.0 | -22 % | yes | 77.9 | 4.85x | yes |
| PNG 3840x2160 | 95.4 | -15 % | 94.7 | -15 % | yes | 362 | 3.80x | yes |
| PNG RGBA (alpha) | 8.27 | -20 % | 8.21 | -20 % | yes | 31.4 | 3.79x | yes |
| WebP lossy | 5.25 | -1 % | 5.20 | -1 % | yes | 17.1 | 3.25x | yes |
| WebP lossless | 16.0 | 0 % | 15.9 | 0 % | yes | 27.8 | 1.74x | yes |
| GIF | 8.13 | -15 % | 8.07 | -15 % | yes | 17.4 | 2.14x | yes |
| BMP | 3.54 | -7 % | 3.51 | -7 % | yes | 13.1 | 3.69x | yes |
| TIFF | 2.81 | -1 % | 2.78 | -1 % | yes | 13.4 | 4.76x | yes |
| batch of ten 1024x768 JPEGs | 6.30 | -2 % | 6.26 | -2 % | yes | 21.2 | 3.36x | yes |

## `qwen3_vl` (second config), milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.50 | -30 % | 0.49 | -30 % | yes | 1.81 | 3.65x | yes |
| JPEG 1024x768 | 3.67 | 0 % | 3.64 | 0 % | yes | 15.0 | 4.09x | yes |
| JPEG 1920x1080 | 10.9 | -18 % | 10.8 | -18 % | yes | 55.1 | 5.05x | yes |
| JPEG 4096x2304 | 74.3 | -1 % | 73.7 | -1 % | yes | 300 | 4.04x | yes |
| JPEG 8192x8192 | 441 | -25 % | 438 | -25 % | yes | 1009 | 2.29x | yes |
| JPEG progressive | 6.76 | -1 % | 6.71 | 0 % | yes | 18.2 | 2.68x | yes |
| JPEG CMYK | 8.16 | -1 % | 8.09 | -1 % | yes | 22.7 | 2.78x | yes |
| JPEG greyscale | 3.22 | -2 % | 3.19 | -2 % | yes | 15.0 | 4.65x | yes |
| JPEG EXIF-rotated | 2.00 | -22 % | 1.98 | -22 % | yes | 6.92 | 3.46x | yes |
| PNG 256x256 | 0.60 | -28 % | 0.59 | -28 % | yes | 2.99 | 4.99x | yes |
| PNG 1024x768 | 5.79 | -12 % | 5.75 | -12 % | yes | 25.3 | 4.37x | yes |
| PNG 1920x1080 | 16.0 | -22 % | 15.9 | -22 % | yes | 78.9 | 4.92x | yes |
| PNG 3840x2160 | 95.8 | -15 % | 95.1 | -15 % | yes | 365 | 3.81x | yes |
| PNG RGBA (alpha) | 8.28 | -20 % | 8.22 | -20 % | yes | 31.3 | 3.78x | yes |
| WebP lossy | 5.22 | -2 % | 5.18 | -1 % | yes | 16.6 | 3.18x | yes |
| WebP lossless | 16.0 | 0 % | 15.9 | 0 % | yes | 28.3 | 1.77x | yes |
| GIF | 8.10 | -15 % | 8.04 | -15 % | yes | 18.2 | 2.24x | yes |
| BMP | 3.62 | -6 % | 3.58 | -6 % | yes | 13.7 | 3.79x | yes |
| TIFF | 2.93 | +4 % | 2.91 | +4 % | yes | 15.1 | 5.15x | yes |
| batch of ten 1024x768 JPEGs | 6.31 | -2 % | 6.26 | -2 % | yes | 21.4 | 3.39x | yes |

## `qwen_vl`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.70 | -42 % | 0.69 | -43 % | yes | 2.16 | 3.09x | yes |
| JPEG 1024x768 | 5.40 | -46 % | 5.36 | -46 % | yes | 18.1 | 3.35x | yes |
| JPEG 1920x1080 | 15.2 | -43 % | 15.0 | -43 % | yes | 61.3 | 4.04x | yes |
| JPEG 4096x2304 | 104 | -32 % | 103 | -32 % | yes | 322 | 3.11x | yes |
| JPEG 8192x8192 | 428 | -23 % | 425 | -23 % | yes | 836 | 1.95x | yes |
| JPEG progressive | 8.38 | -37 % | 8.31 | -37 % | yes | 21.7 | 2.58x | yes |
| JPEG CMYK | 9.97 | -32 % | 9.89 | -33 % | yes | 24.2 | 2.43x | yes |
| JPEG greyscale | 4.97 | -49 % | 4.93 | -49 % | yes | 16.6 | 3.34x | yes |
| JPEG EXIF-rotated | 2.76 | -43 % | 2.73 | -43 % | yes | 7.40 | 2.68x | yes |
| PNG 256x256 | 0.80 | -39 % | 0.78 | -40 % | yes | 3.15 | 3.95x | yes |
| PNG 1024x768 | 7.57 | -41 % | 7.47 | -42 % | yes | 27.6 | 3.64x | yes |
| PNG 1920x1080 | 20.3 | -40 % | 20.1 | -40 % | yes | 84.2 | 4.15x | yes |
| PNG 3840x2160 | 114 | -33 % | 113 | -33 % | yes | 381 | 3.33x | yes |
| PNG RGBA (alpha) | 9.92 | -40 % | 9.84 | -41 % | yes | 33.1 | 3.34x | yes |
| WebP lossy | 6.93 | -40 % | 6.88 | -40 % | yes | 19.4 | 2.80x | yes |
| WebP lossless | 17.7 | -21 % | 17.6 | -21 % | yes | 29.9 | 1.69x | yes |
| GIF | 9.83 | -39 % | 9.74 | -39 % | yes | 19.3 | 1.97x | yes |
| BMP | 5.42 | -47 % | 5.37 | -47 % | yes | 15.3 | 2.82x | yes |
| TIFF | 4.63 | -50 % | 4.59 | -50 % | yes | 15.1 | 3.26x | yes |
| batch of ten 1024x768 JPEGs | 8.34 | -37 % | 8.28 | -37 % | yes | 24.0 | 2.88x | yes |

## `qwen_vl` (second config), milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 | reference ms per image | reference over head | the reference's bytes |
|---|---|---|---|---|---|---|---|---|
| JPEG 256x256 | 0.69 | -42 % | 0.68 | -42 % | yes | 2.09 | 3.01x | yes |
| JPEG 1024x768 | 5.39 | -45 % | 5.35 | -45 % | yes | 17.2 | 3.19x | yes |
| JPEG 1920x1080 | 14.9 | -44 % | 14.7 | -44 % | yes | 59.5 | 4.01x | yes |
| JPEG 4096x2304 | 104 | -33 % | 103 | -33 % | yes | 321 | 3.09x | yes |
| JPEG 8192x8192 | 427 | -24 % | 424 | -24 % | yes | 833 | 1.95x | yes |
| JPEG progressive | 8.52 | -34 % | 8.44 | -35 % | yes | 20.4 | 2.40x | yes |
| JPEG CMYK | 9.92 | -32 % | 9.85 | -32 % | yes | 23.1 | 2.33x | yes |
| JPEG greyscale | 4.85 | -49 % | 4.80 | -49 % | yes | 16.1 | 3.33x | yes |
| JPEG EXIF-rotated | 2.75 | -43 % | 2.73 | -43 % | yes | 7.89 | 2.87x | yes |
| PNG 256x256 | 0.78 | -40 % | 0.77 | -41 % | yes | 3.14 | 4.02x | yes |
| PNG 1024x768 | 7.46 | -42 % | 7.38 | -42 % | yes | 27.2 | 3.64x | yes |
| PNG 1920x1080 | 20.2 | -40 % | 20.1 | -40 % | yes | 85.5 | 4.24x | yes |
| PNG 3840x2160 | 116 | -32 % | 115 | -32 % | yes | 377 | 3.26x | yes |
| PNG RGBA (alpha) | 9.94 | -40 % | 9.87 | -40 % | yes | 33.7 | 3.39x | yes |
| WebP lossy | 7.01 | -39 % | 6.95 | -39 % | yes | 19.5 | 2.78x | yes |
| WebP lossless | 17.7 | -21 % | 17.6 | -21 % | yes | 29.7 | 1.67x | yes |
| GIF | 9.78 | -37 % | 9.70 | -37 % | yes | 18.5 | 1.90x | yes |
| BMP | 5.22 | -49 % | 5.18 | -49 % | yes | 16.1 | 3.07x | yes |
| TIFF | 4.40 | -52 % | 4.33 | -52 % | yes | 15.0 | 3.40x | yes |
| batch of ten 1024x768 JPEGs | 8.16 | -37 % | 8.10 | -37 % | yes | 23.9 | 2.93x | yes |

## `step3_vl`, milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 3.28 | +3 % | 3.25 | +3 % | yes |
| JPEG 1024x768 | 10.6 | -1 % | 10.6 | -1 % | yes |
| JPEG 1920x1080 | 22.2 | -4 % | 22.0 | -4 % | yes |
| JPEG 4096x2304 | 88.1 | -3 % | 87.5 | -3 % | yes |
| JPEG 8192x8192 | 388 | -1 % | 386 | -1 % | yes |
| JPEG progressive | 13.1 | -2 % | 13.0 | -2 % | yes |
| JPEG CMYK | 14.8 | 0 % | 14.7 | 0 % | yes |
| JPEG greyscale | 9.77 | -2 % | 9.69 | -3 % | yes |
| JPEG EXIF-rotated | 5.03 | -9 % | 4.98 | -9 % | yes |
| PNG 256x256 | 2.71 | +6 % | 2.69 | +6 % | yes |
| PNG 1024x768 | 12.4 | -6 % | 12.3 | -6 % | yes |
| PNG 1920x1080 | 27.4 | -9 % | 27.2 | -9 % | yes |
| PNG 3840x2160 | 108 | -9 % | 107 | -9 % | yes |
| PNG RGBA (alpha) | 14.7 | -13 % | 14.6 | -14 % | yes |
| WebP lossy | 11.6 | -2 % | 11.6 | -2 % | yes |
| WebP lossless | 22.4 | -1 % | 22.2 | -1 % | yes |
| GIF | 14.7 | -9 % | 14.5 | -9 % | yes |
| BMP | 10.4 | -2 % | 10.3 | -2 % | yes |
| TIFF | 9.45 | -3 % | 9.38 | -3 % | yes |
| batch of ten 1024x768 JPEGs | 16.1 | -1 % | 16.0 | -1 % | yes |

## `step3_vl` (model type `step5_vl`), milliseconds per image

| class | 1.16.0, one thread | vs 1.15.0 | CPU ms per image | vs 1.15.0 | same bytes as 1.15.0 |
|---|---|---|---|---|---|
| JPEG 256x256 | 3.29 | -3 % | 3.27 | -3 % | yes |
| JPEG 1024x768 | 10.5 | -3 % | 10.4 | -3 % | yes |
| JPEG 1920x1080 | 22.5 | -4 % | 22.3 | -4 % | yes |
| JPEG 4096x2304 | 88.9 | -3 % | 88.3 | -3 % | yes |
| JPEG 8192x8192 | 387 | -2 % | 385 | -2 % | yes |
| JPEG progressive | 13.2 | -2 % | 13.1 | -2 % | yes |
| JPEG CMYK | 14.8 | -1 % | 14.7 | -1 % | yes |
| JPEG greyscale | 9.89 | -2 % | 9.81 | -2 % | yes |
| JPEG EXIF-rotated | 5.11 | -9 % | 5.07 | -10 % | yes |
| PNG 256x256 | 2.83 | +1 % | 2.80 | +1 % | yes |
| PNG 1024x768 | 12.4 | -7 % | 12.3 | -7 % | yes |
| PNG 1920x1080 | 27.5 | -10 % | 27.3 | -10 % | yes |
| PNG 3840x2160 | 108 | -9 % | 108 | -9 % | yes |
| PNG RGBA (alpha) | 14.7 | -14 % | 14.6 | -14 % | yes |
| WebP lossy | 11.8 | -2 % | 11.7 | -2 % | yes |
| WebP lossless | 22.4 | -1 % | 22.3 | -1 % | yes |
| GIF | 14.6 | -11 % | 14.5 | -11 % | yes |
| BMP | 10.4 | -2 % | 10.3 | -2 % | yes |
| TIFF | 9.67 | -2 % | 9.59 | -2 % | yes |
| batch of ten 1024x768 JPEGs | 16.4 | 0 % | 16.2 | 0 % | yes |
