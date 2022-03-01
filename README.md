# OverlapTransformer

The code for our paper submitted to IROS 2022:  

**OverlapTransformer: An Efficient and Rotation-Invariant Transformer Network for LiDAR-Based Place Recognition.**  

OverlapTransformer is a novel lightweight neural network exploiting the range image representation of LiDAR sensors to achieve fast execution with less than 4 ms per frame.  

<img src="https://github.com/haomo-ai/OverlapTransformer/blob/master/query_database.gif" >  

Fig. 1 An animation for finding the top1 candidate with **OverlapTransformer** on sequence 1-1 (database) and 1-3 (query) of **Haomo dataset**.

<img src="https://github.com/haomo-ai/OverlapTransformer/blob/master/haomo_dataset.png" >  

Fig. 2 **Haomo dataset** which is collected by **HAOMO.AI** will be released soon. 


## Dependencies

We are using pytorch-gpu for neural networks.

A nvidia GPU is needed for faster retrival.
OverlapTransformer (OT) is also fast enough when using the neural network on CPU.

To use a GPU, first you need to install the nvidia driver and CUDA.

- CUDA Installation guide: [link](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

- System dependencies:

  ```bash
  sudo apt-get update 
  sudo apt-get install -y python3-pip python3-tk
  sudo -H pip3 install --upgrade pip
  ```

- Python dependencies (may also work with different versions than mentioned in the requirements file)

  ```bash
  sudo -H pip3 install -r requirements.txt
  ```
  
## How to use
We currently only provide the training and test tutorials for KITTI sequences in this repository. The tutorials for Haomo dataset will be open-source once Haomo dataset is released.  

For a quick test of the training and testing procedures, you could use our [pretrained model]().  

### Data structure

We will recommend you follow our data structure.

#### OT structure


```bash
├── more_chosen_normalized_data_1208_1_01.npy
├── config
│   ├── config_haomo.yml
│   └── config.yml
├── modules
│   ├── loss.py
│   ├── netvlad.py
│   ├── overlap_transformer_haomo.py
│   └── overlap_transformer.py
├── test
│   ├── gt_0.3overlap_1.2tau_bet_two_traj.npy
│   ├── test_haomo_topn_prepare.py
│   ├── test_haomo_topn.py
│   ├── test_kitti00_PR_prepare.py
│   ├── test_kitti00_PR.py
│   ├── test_results_haomo
│   │   ├── predicted_des_L2_dis_bet_traj_forward.npz
│   │   └── recall_list.npy
│   └── test_results_kitti
│       └── predicted_des_L2_dis.npz
├── tools
│   ├── read_all_sets.py
│   ├── read_samples_haomo.py
│   ├── read_samples.py
│   └── utils
│       ├── gen_curv_data.py
│       ├── gen_depth_data.py
│       ├── gen_intensity_data.py
│       ├── gen_normal_data.py
│       ├── gen_semantic_data.py
│       ├── __init__.py
│       ├── normalize_data.py
│       ├── split_train_val.py
│       └── utils.py
├── train
│   ├── training_overlap_transformer_haomo.py
│   └── training_overlap_transformer_kitti.py
├── valid
│   └── valid_seq.py
├── visualize
│   ├── des_list.npy
│   └── viz_haomo.py
└── weights
    ├── pretrained_overlap_transformer_haomo.pth.tar
    └── pretrained_overlap_transformer.pth.tar
```
#### Dataset structure
In the file [config.yaml](https://github.com/haomo-ai/OverlapTransformer/blob/master/config/config.yml), the parameters of `data_root` are described as follows:
```
  data_root_folder (KITTI sequences root) follows:
  ├── 00
  │   ├── depth_map
  │     ├── 000000.png
  │     ├── 000001.png
  │     ├── 000002.png
  │     ├── ...
  │   ├── normal_map
  │   ├── overlaps
  │   └── poses_withloop.txt
  ├── 01
  ├── 02
  ├── ...
  └── 10
  
  valid_scan_folder (KITTI sequence 02 velodyne) follows:
  ├── 000000.bin
  ├── 000001.bin
  ...

  gt_valid_folder (KITTI sequence 02 computed overlaps) follows:
  ├── 02
  │   ├── overlap_0.npy
  │   ├── overlap_10.npy
  ...
```
You can find `gt_valid_folder` for sequence 02 [here]().  


### Training

In the file [config.yaml](https://github.com/haomo-ai/OverlapTransformer/blob/master/config/config.yml), `training_seqs` are set for the KITTI sequencs used for training.  

You can start the training with

```
cd train
python ./training_overlap_transformer_kitti.py
```
You can resume from our pretrained model [here](https://github.com/haomo-ai/OverlapTransformer/blob/86cd4a53e1be7029de445ec8b2f3d0fbdb8d38c4/train/training_overlap_transformer_kitti.py#L53) for training.


### Testing

Once a model has been trained , the performance of the network can be evaluated. Before testing, the parameters shoud be set in [config.yaml](https://github.com/haomo-ai/OverlapTransformer/blob/master/config/config.yml)

- `test_seqs`: sequence number for evaluation which is "00" in our work.
- `test_weights`: path of the pretrained model.
- `gt_file`: path of the ground truth file provided by the author of OverlapNet, which can be downloaded [here]().


Therefore you can start the testing scripts as follows:

```
cd test
python test_kitti00_PR_prepare.py
python test_kitti00_PR.py
```
After you run `test_kitti00_PR_prepare.py`, a file named `predicted_des_L2_dis.npz` is generated in `test_results_kitti`, which is used by `python test_kitti00_PR.py`

### visualization
We provide a visualization demo for Haomo dataset To be provided (Fig. 2). Please download the [descriptors]() firstly and then:
```
cd visualize
python viz_haomo.py
```



### License
Coming soon ...
