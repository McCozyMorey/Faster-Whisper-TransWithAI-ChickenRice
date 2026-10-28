# 🎙️ Faster Whisper TransWithAI ChickenRice

[![GitHub Release](https://img.shields.io/github/v/release/TransWithAI/Faster-Whisper-TransWithAI-ChickenRice)](https://github.com/TransWithAI/Faster-Whisper-TransWithAI-ChickenRice/releases)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

High-performance audio/video transcription and translation, optimized for Japanese-to-Chinese translation with Faster Whisper and voice-optimized VAD.

## ⚠️ Important Notice

> This software is completely free and open source.
>
> Repository: https://github.com/TransWithAI/Faster-Whisper-TransWithAI-ChickenRice
>
> Development team: AI Localization Team (https://t.me/transWithAI)

## 🙏 Acknowledgments

- Built on [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper).
- Uses the Japanese-to-Chinese optimized [chickenrice0721/whisper-large-v2-translate-zh-v0.2-st-ct2](https://huggingface.co/chickenrice0721/whisper-large-v2-translate-zh-v0.2-st-ct2) model.
- Supports the Japanese transcription bf16 model [TransWithAI/whisper-ja-1.5B-ct2](https://huggingface.co/TransWithAI/whisper-ja-1.5B-ct2), converted from [efwkjn/whisper-ja-1.5B](https://huggingface.co/efwkjn/whisper-ja-1.5B).
- Uses the voice-optimized [TransWithAI/Whisper-Vad-EncDec-ASMR-onnx](https://huggingface.co/TransWithAI/Whisper-Vad-EncDec-ASMR-onnx) VAD model.
- Thanks to [@Randomless](https://github.com/Randomless) for Modal cloud inference support.
- **Special thanks to an anonymous community member for computing resources and technical support.**

## ✨ Features

- **Accurate Japanese-to-Chinese translation:** ChickenRice v2, trained on 5,000 hours of audio.
- **GPU acceleration:** NVIDIA CUDA 11.8/12.2/12.8 and AMD ROCm/HIP (RDNA1–RDNA4).
- **Cloud inference:** Modal GPU inference without a local graphics card.
- **Multiple subtitle formats:** SRT, VTT, LRC, and more.
- **Audio and video:** Common audio formats (MP3/WAV/FLAC) and video formats (MP4/MKV/AVI).
- **Smart caching:** Automatically skip processed files for efficient batch processing.
- **Flexible configuration:** Customize transcription parameters for different tasks.

## 📦 Package Variants

### No-Model Package

- All GPU dependencies.
- Voice-optimized VAD model.
- whisper-base feature-extraction files for offline use.
- Both translation and transcription `.bat` launchers.
- Main Whisper model **not included**; download or supply it separately.

The default `.bat` files load the main model from the root of `models/`. For a model stored in a subdirectory, add `--model_name_or_path="models\model-folder"` to the relevant launcher.

### Translate Package

- All GPU dependencies, the voice-optimized VAD model, and offline whisper-base feature-extraction files.
- Ready-to-use **ChickenRice v2 (5,000 hours)** Japanese-to-Chinese model.
- Translation `.bat` launchers only.

### Transcribe Package

- All GPU dependencies, the voice-optimized VAD model, and offline whisper-base feature-extraction files.
- Ready-to-use **TransWithAI/whisper-ja-1.5B-ct2** Japanese transcription bf16 model.
- Transcription `.bat` launchers only.

## 🚀 Quick Start

### 1. Choose a CUDA Version

Run `nvidia-smi` to check your CUDA version.

| GPU series | Recommended CUDA version |
| --- | --- |
| GTX 10/16 | CUDA 11.8 |
| RTX 20/30 | CUDA 11.8 or 12.2 |
| RTX 40 | CUDA 12.2 or 12.8 |
| RTX 50 | **CUDA 12.8 required** |

### AMD GPUs (ROCm/HIP)

On Windows, download the ZIP whose `gfx***` suffix matches your GPU architecture. ROCm/HIP runtime DLLs are bundled; a separate ROCm installation is generally unnecessary.

| GPU | Download suffix | GFX architecture |
| --- | --- | --- |
| RX 5000 / RDNA1 | `gfx101x_dgpu` | gfx1010 / gfx1011 / gfx1012 |
| RX 6000 / RDNA2 | `gfx103x_dgpu` | gfx1030 / gfx1031 / gfx1032 / gfx1034 |
| RX 7000 / RDNA3 | `gfx110x_all` | gfx1100 / gfx1101 / gfx1102 (some iGPUs use gfx1103) |
| RX 9000 / RDNA4 | `gfx120x_all` | gfx1200 / gfx1201 |

> **Don't know your GPU model?** On Windows, open Task Manager → Performance → GPU, or Device Manager → Display adapters.

**Quick self-check**

| GPU model | Download suffix |
| --- | --- |
| RX 5300 / RX 5500 / RX 5600 / RX 5700 series | `gfx101x_dgpu` |
| RX 6400 / RX 6500 XT / RX 6600 / RX 6700 / RX 6800 / RX 6900 series | `gfx103x_dgpu` |
| RX 7600 / RX 7700 XT / RX 7800 XT / RX 7900 series | `gfx110x_all` |
| RX 9060 / RX 9060 XT / RX 9070 (including GRE/XT) | `gfx120x_all` |
| Radeon 890M / 8060S / 860M iGPU (gfx115x) | Not yet supported |

> iGPU users should use the CPU build or Modal cloud inference.

<details>
<summary><b>Full model lists (by series)</b></summary>

- **RX 5000 (RDNA1)** -> `gfx101x_dgpu`
  - Desktop: RX 5300, RX 5300 XT, RX 5500, RX 5500 XT, RX 5600, RX 5600 XT, RX 5700, RX 5700 XT (incl. 50th Anniversary Edition)
  - Mobile dGPU: RX 5300M, RX 5500M, RX 5600M, RX 5700M

- **RX 6000 (RDNA2)** -> `gfx103x_dgpu`
  - Desktop: RX 6300 (OEM), RX 6400, RX 6500 XT, RX 6600, RX 6600 XT, RX 6650 XT, RX 6700, RX 6700 XT, RX 6750 GRE, RX 6750 XT, RX 6800, RX 6800 XT, RX 6900 XT, RX 6950 XT
  - Mobile dGPU: RX 6300M, RX 6450M, RX 6500M, RX 6550S, RX 6550M, RX 6600S, RX 6600M, RX 6650M, RX 6650M XT, RX 6700S, RX 6700M, RX 6800S, RX 6800M, RX 6850M XT

- **RX 7000 (RDNA3)** -> `gfx110x_all`
  - Desktop: RX 7400 (OEM), RX 7600, RX 7600 XT, RX 7650 GRE, RX 7700 (OEM), RX 7700 XT, RX 7800 XT, RX 7900 GRE, RX 7900 XT, RX 7900 XTX
  - Mobile dGPU: RX 7600S, RX 7600M XT, RX 7700S, RX 7800M, RX 7900M

- **RX 9000 (RDNA4)** -> `gfx120x_all`
  - Desktop: RX 9060, RX 9060 XT, RX 9070 GRE, RX 9070, RX 9070 XT

</details>

Usage is the same as NVIDIA builds. Run the translation or transcription GPU launcher listed below. Internally, these use `--device=cuda`, following CTranslate2's HIP convention. The CLI also accepts `--device=amd` as an alias for `--device=cuda`.

### 2. Download

Download the appropriate package from [Releases](https://github.com/TransWithAI/Faster-Whisper-TransWithAI-ChickenRice/releases).

### 3. Run

Drag audio or video files onto the appropriate batch file. **The filenames below intentionally match the distributed files, which use Chinese names.**

```text
# Translation: GPU mode (recommended, at least 6 GB VRAM)
运行(翻译)(GPU).bat

# Translation: low-VRAM GPU mode (int8_float16, 4 GB VRAM)
运行(翻译)(GPU,低显存模式).bat

# Translation: CPU mode
运行(翻译)(CPU).bat

# Transcription: GPU mode
运行(转录)(GPU).bat

# Transcription: CPU mode
运行(转录)(CPU).bat
```

## ☁️ Modal Cloud Inference

Use a Modal cloud GPU if you lack a local GPU or sufficient VRAM.

### 1. Set Up the Environment

```bash
# Use the existing Conda environment with Modal support
conda activate faster-whisper-cu118  # or cu122 / cu128

# Or install manually
pip install modal questionary
```

### 2. Configure Your Modal Account

Register at https://modal.com/. The original README advertised $30 in monthly free credit for new users; check Modal for current availability.

```bash
modal token new
```

### 3. Run Cloud Inference

```bash
# Packaged executable
modal_infer.exe

# Or Python
python modal_infer.py
```

The program interactively asks for the GPU type, model, input files, and other parameters. The original recommended configuration is a T4 GPU for general transcription tasks and cost efficiency.

> This project is not affiliated with Modal. For sponsorship inquiries, open an issue.

See the Modal cloud inference section in the [user guide](使用说明.txt) for details.

## 📖 Documentation

- [User guide](使用说明.txt): detailed instructions and parameter configuration.
- [Release notes](RELEASE_NOTES_CN.md): changes and package-selection guidance.
- [Generation configuration](generation_config.json5): transcription parameters.

## 🛠️ Advanced Configuration

### Command-Line Options

Edit the batch file and append options after `infer.exe`:

```text
# Overwrite existing subtitles
--overwrite

# Output directory
--output_dir="path"

# Audio and subtitle formats
--audio_suffixes="mp3,wav"
--sub_formats="srt,vtt,lrc"

# Logging level
--log_level="INFO"

# Override the Whisper task (translate or transcribe)
--task="transcribe"
```

### Transcription Parameters

Edit `generation_config.json5` to configure transcription. Set `"task": "translate"` or `"task": "transcribe"`; the command-line `--task` overrides the configuration file.

See the [Faster Whisper parameter documentation](https://github.com/SYSTRAN/faster-whisper/blob/dea24cbcc6cbef23ff599a63be0bbb647a0b23d6/faster_whisper/transcribe.py#L733).

**Subtitle merging and deduplication (`segment_merge`)**

- Merge repeated or overlapping segments to reduce duplicate subtitles.
- If a subtitle lasts unusually long, reduce `segment_merge.max_gap_ms` or `segment_merge.max_duration_ms`, or set `segment_merge.enabled` to `false`.

## 🔗 Related Links

- **Faster Whisper:** https://github.com/SYSTRAN/faster-whisper
- **ChickenRice model:** https://huggingface.co/chickenrice0721/whisper-large-v2-translate-zh-v0.2-st-ct2
- **TransWithAI Japanese transcription model:** https://huggingface.co/TransWithAI/whisper-ja-1.5B-ct2
- **Original Japanese transcription model:** https://huggingface.co/efwkjn/whisper-ja-1.5B
- **Voice-optimized VAD model:** https://huggingface.co/TransWithAI/Whisper-Vad-EncDec-ASMR-onnx
- **OpenAI Whisper:** https://github.com/openai/whisper
- **Modal:** https://modal.com/
- **AI Localization Team:** https://t.me/transWithAI

## 💡 FAQ

**GPU mode does not work?** Update your graphics driver. NVIDIA users should check CUDA compatibility; AMD users should check that the downloaded package matches their `gfx` architecture.

**No subtitles generated?** Check the supported file formats and console errors, then try `--overwrite`.

**Not enough RAM or VRAM?** Use low-VRAM mode, CPU mode, or Modal cloud inference.

**Which CUDA version should I choose?** Run `nvidia-smi` and consult the compatibility table in the [release notes](RELEASE_NOTES_CN.md).

## 📞 Support

1. Read the [user guide](使用说明.txt) and [release notes](RELEASE_NOTES_CN.md).
2. Check that your graphics driver is up to date.
3. Confirm that you selected the appropriate CUDA version.
4. Open an issue in the project repository.

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=TransWithAI/Faster-Whisper-TransWithAI-ChickenRice&type=Date)](https://star-history.com/#TransWithAI/Faster-Whisper-TransWithAI-ChickenRice&Date)

## 📄 License

Licensed under MIT. See [LICENSE](LICENSE).

---

Built on Faster Whisper. The ChickenRice model was optimized with 5,000 hours of audio for Japanese-to-Chinese translation. Open-source maintenance by the AI Localization Team; permanently free.

**Special thanks again to the anonymous community member for computing resources and technical support!**
