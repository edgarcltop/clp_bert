# Clpbert

 Clpbert, an efficient framework for 
end-to-end learning for image-text and video-text tasks. 
It takes raw videos/images + text as inputs, and outputs task predictions.
ClpBERT is designed based on 2D CNNs and transformers, and uses a sparse sampling strategy 
to enable efficient end-to-end video-and-language learning. In this repository, 
we support end-to-end pretraining and finetuning for the following tasks:

- Image-text pretraining on COCO and VG captions.
- Text-to-video retrieval finetuning on MSRVTT, DiDeMo, and ActivityNet Captions.
- Video-QA finetuning on TGIF-QA and MSRVTT-QA.

It is also feasible and easy to add other image-text or video-text tasks for pretraining and finetuning. 


## Requirements 
We provide a Docker image for easier reproduction. Please install the following:
  - [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (19.03+), 

## Getting Started

### General

1. Create a folder that stores pretrained models, all the data, and results.
    ```bash
    PATH_TO_STORAGE=/path/to/your/data/
    mkdir -p $PATH_TO_STORAGE/txt_db  # annotations
    mkdir -p $PATH_TO_STORAGE/vis_db  # image and video 
    mkdir -p $PATH_TO_STORAGE/finetune  # finetuning results
    mkdir -p $PATH_TO_STORAGE/pretrained  # pretrained models
    ```

2. Download pretrained models.

    Our e2e pretrained ClipBERT model (849MB), can be downloaded with the following command.
    ```bash
    bash scripts/download_pretrained.sh $PATH_TO_STORAGE
    ```
    This pretrained model can be used for finetuning on video-text tasks and image-text tasks.
    For your convenience, this script will also download `bert-base-uncased` and `grid-feat-vqa` 
    model weights, which are used as initialization for pretraining.  

3. Launch the Docker container for running the experiments.
    ```bash
    # docker image should be automatically pulled
    source launch_container.sh $PATH_TO_STORAGE/txt_db $PATH_TO_STORAGE/vis_db \
        $PATH_TO_STORAGE/finetune $PATH_TO_STORAGE/pretrained
    ```
    The launch script respects $CUDA_VISIBLE_DEVICES environment variable.
    Note that the source code is mounted into the container under `/clipbert` instead 
    of built into the image so that user modification will be reflected without
    re-building the image. (Data folders are mounted into the container separately
    for flexibility on folder structures.)
