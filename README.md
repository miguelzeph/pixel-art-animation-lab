# Pixel Art Animation Lab

Local AI-assisted workflow for creating consistent pixel-art characters and animations for HTML5 games.

## Hardware and Environment

- **OS:** Ubuntu 26.04.1 LTS
- **GPU:** AMD Radeon 780M Graphics (`gfx1103`)
- **CPU:** AMD Ryzen 7 PRO 250
- **Python:** 3.12.3
- **PyTorch:** 2.14.0+rocm10.1.0
- **GPU backend:** ROCm/HIP

## Project Structure

```text
pixel-art-animation-lab/
├── ComfyUI/                     # ComfyUI Git submodule
├── .venv/                       # Python virtual environment (not tracked)
├── models/                      # Model documentation
├── input/                       # Source character images
├── output/                      # Generated images and animations
├── workflows/                   # ComfyUI workflow files
├── comfyui-requirements.txt     # ComfyUI dependencies excluding PyTorch
├── requirements.txt             # PyTorch ROCm dependencies
├── README.md
└── .gitignore
```

## Python Environment

The project uses a Python virtual environment in the repository root.

Activate it with:

```bash
source .venv/bin/activate
```

## PyTorch and AMD GPU

PyTorch is installed with ROCm support for the AMD Radeon 780M.

Verify GPU availability:

```bash
python -c "import torch; print(torch.__version__); print('HIP:', torch.version.hip); print('GPU available:', torch.cuda.is_available()); print(torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'GPU not detected')"
```

Expected result:

- PyTorch: `2.14.0+rocm10.1.0`
- HIP: a reported version
- GPU available: `True`
- GPU: `AMD Radeon 780M Graphics`

The `torch.cuda` API is used by PyTorch for HIP-enabled AMD GPUs as well.

## Installing Dependencies

Install the PyTorch dependencies from the project root:

```bash
pip install -r requirements.txt
```

Install the remaining ComfyUI dependencies without allowing its requirements file to replace PyTorch:

```bash
cd ComfyUI
grep -Ev '^(torch|torchvision|torchaudio)([<=>!~; ]|$)' requirements.txt > ../comfyui-requirements.txt
cd ..
pip install -r comfyui-requirements.txt
```

**Important:** Keep the ROCm-enabled PyTorch installation. Do not install the default CUDA build intended for NVIDIA GPUs.

## Starting ComfyUI

```bash
cd ComfyUI
source ../.venv/bin/activate
python main.py --cpu-vae --lowvram
```

Open the interface in a browser:

http://127.0.0.1:8188

Keep the terminal open while using ComfyUI.

## Workflow

1. Place source character images in `input/`.
2. Load or create workflows in `workflows/`.
3. Store AI-generated models locally in the appropriate model directory.
4. Generate images and animation frames.
5. Edit and assemble frames using GIMP or another pixel-art animation editor.
6. Export final assets for the HTML5 game.

## Git and Large Files

- ComfyUI is managed as a Git submodule.
- AI model weights and generated assets should remain local unless explicitly selected for version control.
- Avoid committing large model files to Git.


## Starting - Stable Diffusion 1.5 Checkpoint

Downloaded the inference-oriented Stable Diffusion 1.5 checkpoint:

v1-5-pruned-emaonly.safetensors

Source:

https://huggingface.co/sd-legacy/stable-diffusion-v1-5

The checkpoint was selected because the EMA-only version uses less storage and memory than the version containing additional non-EMA weights.

Created the ComfyUI checkpoint directory:

```bash
mkdir -p ~/Dev/pixel-art-animation-lab/ComfyUI/models/checkpoints
```

Moved the downloaded checkpoint into that directory:

```bash
mv ~/Downloads/v1-5-pruned-emaonly.safetensors \
  ~/Dev/pixel-art-animation-lab/ComfyUI/models/checkpoints/
```

The model is stored locally and should not be committed to Git.

5. Starting the ComfyUI Server

The ComfyUI server was successfully started without errors using:

```bash
cd ~/Dev/pixel-art-animation-lab/ComfyUI
source ../.venv/bin/activate
python main.py --cpu-vae --lowvram
```

The interface is available locally at:

http://127.0.0.1:8188

The --cpu-vae and --lowvram options are intended to reduce GPU memory pressure on the integrated Radeon 780M.


## Tips
- seed: controla aleatoriedade
- Steps -> se diminuir, reduz o tempo de geração.
- denoise (0-1) -> quanto menor, menos a imagem muda da imagem de input
- cfg -> influência do prompt
- positive -> é a descrição que você deseja criar
- negative -> são as característticas que voce quer evitar
