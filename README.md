# PyTorch Video Dataset Loader

This repository contains a PyTorch-compatible dataset loader for video classification tasks. It is designed to load video datasets, convert them into frames, and provide them as input to PyTorch models. This loader is particularly useful for tasks like Indian Sign Language (ISL) recognition and other video classification problems.

## Requirements

To run the notebook, ensure you have the following installed:

- `torch`
- `torchvision`
- `PIL` (Python Imaging Library)
- `subprocess`
- `shutil`
- `os`
- `sys`
- `FFmpeg`

## Dataset Structure

The dataset should be structured as follows:

```
root/
├── dog/
│   ├── xxx.mp4
│   ├── xxy.mp4
│   └── ...
├── cat/
│   ├── 123.mp4
│   ├── nsdf3.mp4
│   └── ...
└── ...
```

Where each folder (dog, cat, etc.) represents a class, and each .mp4 file represents a video belonging to that class.

## How to Use

To use the dataset loader, follow these steps:

### 1. Prepare Your Data

Organize your videos into class folders (e.g., dog/, cat/), where each folder contains videos for that class. You can follow the dataset structure described above.

### 2. Initialize the Dataset Loader

Import the necessary libraries and initialize the `VideoDataset` class with your data path and required parameters.

```
from Video_dataset_loader import VideoDataset, data_and_words

# Define your data path
data_path = '/path/to/your/dataset'

# Load the data
data, words_list, words_dict = data_and_words(data_path)

# Initialize the dataset
dataset = VideoDataset(
    data=data,
    temp_data_folder='/path/to/temp/folder',
    NUM_FRAMES=10,  # Number of frames to extract per video
    transform_frame=None,  # Optional: Transform to apply on individual frames
    transform_video=None,  # Optional: Transform to apply on the entire video
    video_fps=25,  # Frames per second for video extraction
    resolution='1920:1080'  # Resolution for video frames
)
```

### 3. Use DataLoader for Batch Processing

You can use PyTorch's DataLoader to load the data in batches.
```
from torch.utils.data import DataLoader

batch_size = 32  # Set your batch size
dataloader = DataLoader(dataset, batch_size=batch_size, shuffle=True)

# Loop through batches
for video_frames, labels in dataloader:
    # Your training code here
    pass
```

### 4. Example Use Case

For an example of how to use this dataset loader in a video classification task, check out the [SignLink-ISL](https://github.com/Naneet/SignLink-ISL) repository, where this loader is used for ISL (Indian Sign Language) recognition.

**Note:** For a deeper understanding of how the dataset loader works, you can also take a look at the `VideoDataset` class itself. The code is well-commented to provide clear explanations and to help optimize its usage.

## Notes
#### Temporary Folder for Frames:

The loader uses a temporary folder to store the frames extracted from the videos. Make sure this folder is empty before use as frames will be deleted after they are processed.

#### FFmpeg:

The loader uses `ffmpeg` to convert videos to frames. Make sure ffmpeg is installed and available in your system's PATH.

#### Transformations:

You can apply transformations to individual frames using the `transform_frame` parameter, and to the entire video using the `transform_video` parameter. It is recommended to use `torchvision.transforms` for common image transformations such as normalization, random cropping, and more. If you decide to use `transform_frame`, ensure that you include `ToTensor()` to convert the frames into tensors.


