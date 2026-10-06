# llama-cpp-python-v0.3.36-0.6.0-b11448-Wheel-Vision-CUDA-13.2-
Pre-Build Wheel for ComfyUI with Python 3.12.14 for Qwen 3.8
# Pre-built High-Performance llama-cpp-python Wheel (Vision & CUDA 13.2)

For v0.0.1 you absolutely need: https://developer.nvidia.com/cuda-13-2-0-download-archive

Build on: Intel i9-13900k, Geforce 4090RTX no AVX512

This repository provides a custom, pre-built Windows x64 binary (`.whl`) for **llama-cpp-python** utilizing the brand-new **llama.cpp b11448 (0.6.0 Core)** engine. 

It is specifically compiled for high-performance **Vision-Language Models (like Qwen3-VL or LLaVA)** under Windows for Endusers. By using this pre-built wheel, you don't need to install heavy build chains like Visual Studio 2026, CMake, or the CUDA Toolkit locally.

## ✨ Features included in this Build

* **Core Engine:** Upgraded to `llama.cpp b11448` (0.6.0 Pre-release) 🛠️
* **CUDA Support:** Fully optimized for **CUDA 13.2** (Windows x64) ⚡
* **Vision/Multimodal:** Native modern image processing enabled via the new `mtmd.dll` architecture 🖼️
* **Turbo Inference:** Enabled native FP16 GPU calculations via `-DGGML_CUDA_F16=ON` 🚀
* **Flash-Attention:** Drastically reduces VRAM consumption and accelerates processing for high-resolution image tokens via `-DGGML_FLASH_ATTN=ON` 🧠
* **Windows Compatibility Fix:** Built with dynamic libraries (`-DBUILD_SHARED_LIBS=ON`) to prevent missing DLL crashes inside virtual environments.

## 💻 System Requirements

To use this wheel seamlessly, your system should match the following stack:
* **Operating System:** Windows 10 / 11 (x64)
* **Python Version:** Python 3.12 (e.g., as used in modern ComfyUI or standalone setups)
* **NVIDIA Driver:** A modern GPU driver supporting CUDA 13.2 runtimes
* **Python 3.12.14

## 📦 Installation Guide

1. Download the `.whl` file from the **Releases** section of this repository.
2. Open your terminal or command prompt (activate your virtual environment/ComfyUI venv if applicable).
3. Run the following command to force-install the wheel:

```bash
pip install llama_cpp_python-0.3.36-py3-none-win_amd64.whl --force-reinstall --no-cache-dir
```

## 🖼️ How to use the new Multimodal Engine (Vision)

Since `llama.cpp 0.6.0+` replaced the legacy LLaVA structure with the new `mtmd.dll` (Multi-Token Multi-Domain) pipeline, make sure to use the modern `MTMDChatHandler` instead of the legacy `llava_cpp` module in your scripts:

```python
from llama_cpp import Llama
from llama_cpp.llama_chat_format import MTMDChatHandler

# 1. Initialize the new Multimodal handler (pointing to your clip/mmproj model)
chat_handler = MTMDChatHandler(clip_model_path="path/to/mmproj-model.gguf")

# 2. Load the main model using the CUDA 13.2 backend
llm = Llama(
    model_path="path/to/qwen3_or_llama_model.gguf",
    chat_handler=chat_handler,
    n_ctx=4096,
    n_gpu_layers=-1 # Pushes all layers to the compiled ggml-cuda.dll
)

print("🚀 Engine successfully initialized and running on GPU via CUDA 13.2!")
```

## 🛠️ Build Configuration Reference
For transparency, this binary was compiled inside the Visual Studio 2026 Developer Command Prompt using the following environment setup:
```cmd
set FORCE_CMAKE=1
set CMAKE_GENERATOR_PLATFORM=x64
set CMAKE_ARGS=-DGGML_CUDA=ON -DLLAMA_LLAVA=ON -DGGML_CUDA_F16=ON -DGGML_FLASH_ATTN=ON -DBUILD_SHARED_LIBS=ON
pip wheel . --wheel-dir=./dist --no-deps --no-cache-dir
```

## 📄 License & Acknowledgments
This binary is distributed under the **MIT License**. 
Special thanks to the authors of [llama-cpp-python](https://github.com/abetlen/llama-cpp-python) and the core development team at [llama.cpp](https://github.com/ggml-org/llama.cpp) for their incredible work.
