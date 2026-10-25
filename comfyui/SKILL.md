# ComfyUI Node Generation

Source: github.com/Comfy-Org/ComfyUI (GPL-3.0). The most powerful and modular AI engine for content creation: a visual node graph for building and reusing image, video, audio, 3D, and text workflows without code. GitHub weekly trending #10, week of 2026-10-25.

## Why it matters for this user

- The user is an AI short-video creator and has a GPU machine. ComfyUI is the local engine for that machine: it runs even the biggest open-source models on as little as 4 GB VRAM plus 8 GB RAM using asynchronous weight streaming, and it is the most optimized local inference engine for diffusion models.
- It does not run on the Muse VM (no GPU). ComfyUI belongs on the user's GPU machine; Muse can write workflows and guides for it, not execute it.

## Install

- Easiest for new users: the desktop application from comfy.org (Windows and macOS).
- Windows portable: download the portable build for the right GPU (NVIDIA 20 series and above, AMD, or Intel variants), extract with 7-Zip or Windows Explorer, and run. Put checkpoints (the large .ckpt / .safetensors files) in `ComfyUI\models\checkpoints`; larger models have multiple files, so follow each model's instructions for which subfolder to use. If extraction fails, right-click the file, Properties, Unblock. The portable build ships Python 3.13 and PyTorch CUDA 13.0; update NVIDIA drivers if it does not start.
- comfy-cli: `pip install comfy-cli`, then `comfy install`.
- Manual install supports Windows, Linux, NVIDIA, AMD, Intel, Apple Silicon, and Ascend NPUs.
- Cloud alternative: Comfy Cloud (official paid cloud version) for users without local hardware.

## Use it

- Open the web UI in a browser. Build a graph by connecting nodes: load a model, write a prompt, sample, decode, save.
- Model support spans image generation (SD1.5, SDXL, SD3.5, Flux.1, Flux.2, Qwen Image, Z-Image, Hunyuan Image 2.1, HiDream, and more), image editing (Flux Kontext, Qwen Image Edit, OmniGen2, and more), video (Wan 2.1 and 2.2, LTX-Video 2 and 2.3, HunyuanVideo 1.5, CogVideoX, and more), audio (ACE-Step 1.5, Stable Audio 3, MiniMax Music 3), and 3D and vision (Hunyuan3D 2.1, TripoSplat, SAM 3, Depth Anything 3, and more).
- Partner nodes provide access to closed-source models such as Nano Banana, Seedance, and Hunyuan3D; use `--offline` to disable them and force all built-in functionality to stay offline, or `--disable-partner-nodes` to only disable the API nodes.
- Built-in tools: inpainting, outpainting, reference conditioning, masks and compositing, model merging, upscaling, frame interpolation, segmentation, depth estimation, and media processing.
- Save and load workflows as JSON, or recover complete workflows and seeds from supported generated media. Reusable subgraphs, workflow templates, App Mode (expose a complex workflow through a simple UI), and a local API for pipeline integration.
- Run fully offline: core downloads nothing unless requested. Configure additional model locations with `extra_model_paths.yaml` (rename the example file and edit it) to share models with another UI.
- Extend with custom nodes via ComfyUI-Manager.

## Shortcuts that matter

- `Ctrl + Enter`: queue current graph for generation.
- `Ctrl + Shift + Enter`: queue current graph as first.
- `Ctrl + Alt + Enter`: cancel current generation.
- `Ctrl + Z` / `Ctrl + Y`: undo / redo. `Ctrl + S`: save workflow. `Ctrl + O`: load workflow. `Ctrl + D`: load default graph.
- `Ctrl + B`: bypass selected nodes. `Ctrl + M`: mute / unmute. `Ctrl + G`: group. `Q`: toggle queue. `Space` (held): pan the canvas. Double-click: node quick search palette.
- On macOS, use `Cmd` in place of `Ctrl`.
