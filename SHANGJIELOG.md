## Installation
 - Clone this repository
   ```
   git clone git@github.com:shangjie-li/OpenPCDet.git
   ```
 - Install PyTorch environment with Anaconda (Tested on Ubuntu 16.04 & CUDA 10.2)
   ```
   conda create -n pcdet.v0.5.0 python=3.6
   conda activate pcdet.v0.5.0
   cd OpenPCDet
   pip install -r requirements.txt
   ```
 - Install spconv
   ```
   # Try the command below:
   pip install spconv-cu102
   
   # If there is `ERROR: Cannot uninstall 'certifi'.`, try:
   pip install spconv-cu102 --ignore-installed
   ```
 - Install pcdet library
   ```
   cd OpenPCDet
   python setup.py develop
   ```
 - Install visualization tools
   ```
   pip install mayavi
   pip install pyqt5
   
   # If you want import opencv, run:
   pip install opencv-python
   
   # If you want import open3d, run:
   pip install open3d-python
   ```

## KITTI3D Dataset (41.5GB)
 - Download KITTI3D dataset: [calib](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_calib.zip), [velodyne](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_velodyne.zip), [label_2](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_label_2.zip) and [image_2](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_image_2.zip).
 - Download [road plane](https://drive.google.com/file/d/1d5mq0RXRnvHPVeKx6Q612z0YRO1t2wAp/view?usp=sharing) for data augmentation.
 - Organize the downloaded files as follows
   ```
   OpenPCDet
   ├── data
   │   ├── kitti
   │   │   │── ImageSets
   │   │   │── training
   │   │   │   ├──calib & velodyne & label_2 & image_2 & planes
   │   │   │── testing
   │   │   │   ├──calib & velodyne & image_2
   ├── pcdet
   ├── tools
   ```
 - Generate the ground truth database and data infos by running the following command
   ```
   # This will create gt_database dir & 5 pkl files in OpenPCDet/data/kitti.
   cd OpenPCDet
   python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml
   ```

## Demo
 - Run the demo with a pretrained model (e.g. [here](https://drive.google.com/file/d/1wMxWTpU1qUoY3DsCH31WJmvJxcjFXKlm/view?usp=sharing))
   ```
   cd OpenPCDet/tools
   python demo.py --cfg_file path_to_your_config_file --ckpt path_to_your_ckpt --data_path path_to_your_data
   
   # For example
   python demo.py --cfg_file cfgs/kitti_models/pointpillar.yaml --ckpt ../pointpillar_7728.pth --data_path ../data/kitti/training/velodyne/000008.bin
   ```

## Training
 - Run the command below to train
   ```
   cd OpenPCDet/tools
   python train.py --cfg_file cfgs/kitti_models/pointpillar.yaml --batch_size 2
   ```

## Evaluation
 - Run the command below to evaluate
   ```
   cd OpenPCDet/tools
   python test.py --cfg_file cfgs/kitti_models/pointpillar.yaml --batch_size 2 --ckpt ../pointpillar_7728.pth
   ```
 - The 3D detection performance on KITTI3D should be
   | Class                | AP (R11) BEV              | AP (R11) 3D               |
   |:--------------------:|:-------------------------:|:-------------------------:|
   | Car (Iou=0.7)        | 89.6590, 87.1725, 84.3762 | 86.4617, 77.2839, 74.6530 |
   | Pedestrian (Iou=0.5) | 61.6348, 56.2747, 52.6007 | 57.7500, 52.2916, 47.9072 |
   | Cyclist (Iou=0.5)    | 82.2593, 66.1110, 62.5585 | 80.0483, 62.6080, 59.5260 |
    * Report in different difficulties, which are Easy, Moderate and Hard.
