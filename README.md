
# MMAGIC-Sam

MMAGIC-Sam is a project that leverages the controlnet_animation model from MMAGIC and the SAM model by Meta to generate AI animations and segment images. The following instructions guide you through the installation process and demonstrate how to use the provided scripts to create animated videos.

## Insall MMAGIC (Follow these steps if you don't have MMAGIC installed already)
## Prerequisites 🛠️

Before you get started, make sure you have the following dependencies installed:

- Python 3.8 (or a compatible version) 🐍
- Miniconda ⚙️

## Installation Steps 📦

Follow these simple steps to set up your environment for using the MMagic Stable Diffusion model:

1. **Install [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) on your system if you haven't already.**

2. **Create a Conda environment and activate it using the following commands:**

   ```bash
   conda create --name mmagic python=3.8 -y
   conda activate mmagic
   ```

3. **Check your CUDA version by running the command:**

   ```bash
   nvcc --version
   ```

4. **Install PyTorch by referring to the [official PyTorch documentation](https://pytorch.org/). Select the appropriate configuration based on your system and follow the installation instructions.**


5. **Install the `MMCV` library using `MIM`, a package manager for AI and machine learning dependencies. Run the following commands:**

   ```bash
   pip install -U openmim
   mim install 'mmcv>=2.0.0'
   ```

6. **Install `mmengine` from the GitHub repository:**

   ```bash
   pip install git+https://github.com/open-mmlab/mmengine.git
   ```

7. **Install the `mmagic` toolbox in editable mode using the following command:**

   ```bash
   git clone https://github.com/open-mmlab/mmagic.git
   cd mmagic
   pip3 install -e . -v
   ```

   The `-e .` flag is used to install the Python package in editable mode, meaning that any changes made to the source code will be reflected in the installed package.


## Installation of SAM

1. Install SAM:

```bash
pip install git+https://github.com/facebookresearch/segment-anything.git
```

2. Install ffmpeg to extract frames from a video:

```bash
sudo apt install ffmpeg
```

3. Install SAM checkpoints:

```bash
mkdir -p checkpoints/sam
wget -O checkpoints/sam/sam_vit_h_4b8939.pth https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth
```

4. Clone this repo

```bash
https://github.com/adrienckr/controlnet-sam
```

## Usage

1. Take a video clip that you want to edit and extract frames:

```bash
mkdir -p frames
ffmpeg -i your_video.mp4 frames/%04d.jpg
```

2. Run the script to generate animated frames using controlnet_animation:

```bash
python play_controlnet_animation_sam.py
```

3. Combine all the frames into a final video using ffmpeg:

```bash
ffmpeg -r 10 -i results/final_frames/%04d.jpg -b:v 30M -vf fps=10 results/final_frames.mp4
```

## Explanation of Steps

The final video is created through the following steps:

1. Split the input video into frames.
2. Use the controlnet_animation model from MMAGIC to modify each frame and generate AI animation.
3. Utilize stable diffusion in MMAGIC to create a background image that complements the semantics of the animation content.
4. Use SAM to predict the mask of the person in the animation.

Note: Backgrounds can be further customized using images generated by stable-diffusion.

Feel free to explore and modify the provided scripts for your specific use case!
