# ComfyUI Deployment


## Quick-Install (Windows + NVIDIA)
There is a portable standalone version of ComfyUI that will let you run ComfyUI with NVIDIA GPU or CPU. CPU generation is very slow, so this may as well be a NVIDIA-only installer:
1. Click this [Download Link](https://github.com/comfyanonymous/ComfyUI/releases/download/latest/ComfyUI_windows_portable_nvidia_cu118_or_cpu.7z) and your download will start.
2. Extract the .zip file with [7-Zip](https://7-zip.org/). You will get a folder called ComfyUI_windows_portable containing the ComfyUI folder.
3. Double click the file run_nvidia_gpu.bat to run with NVIDIA GPU, or run_cpu.bat to run with CPU.


## Clone from Github (Windows/Linux)

### NVIDIA GPU
1. Open your Terminal
2. Then run the follow commands one by one:
    ```
    git clone https://github.com/comfyanonymous/ComfyUI
    cd ComfyUI
    pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu118 xformers -r requirements.txt
    ```
3. You can now launch ComfyUI with the command:
    ```
    python main.py
    ```
4. If you get the "Torch not compiled with CUDA enabled" error, uninstall torch (when prompted, press "y") and install it again with the same command as before:
    ```
    pip uninstall torch
    pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu118 xformers -r requirements.txt
    ```

### AMD GPU
1. Open your Terminal
2. Then run the follow commands one by one:
    ```
    git clone https://github.com/comfyanonymous/ComfyUI
    cd ComfyUI
    python -m pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/rocm5.4.2 -r requirements.txt
    ```
3. If that fails because your GPU isn't officially supported by ROCm (6700 XT , etc..), you can try running the command:
    ```
    HSA_OVERRIDE_GFX_VERSION=10.3.0 python main.py
    ```
4. You can now launch ComfyUI with the command:
    ```
    python main.py
    ```


## Updating ComfyUI

### Quick-Installed
Not available

### Cloned
Navigate to the ComfyUI folder in your Command Prompt/Terminal and type:
```
git pull
```