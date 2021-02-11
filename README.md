# One-Shot Learning With Attention-Guided Segmentation in Cryo-Electron Tomography

Bo Zhou, Haisu Yu, Xiangrui Zeng, Xiaoyan Yang, Jing Zhang, and Min Xu

Frontiers in Molecular Biosciences (AI in Biological and Biomedical Imaging), 2020

[[Paper](https://www.frontiersin.org/articles/10.3389/fmolb.2020.613347/full)]

This repository contains the PyTorch implementation of COS-Net.

### Citation
If you use this code for your research or project, please cite:

    @article{zhou2020one,
      title={One-Shot Learning With Attention-Guided Segmentation in Cryo-Electron Tomography},
      author={Zhou, Bo and Yu, Haisu and Zeng, Xiangrui and Yang, Xiaoyan and Zhang, Jing and Xu, Min},
      journal={Frontiers in Molecular Biosciences},
      volume={7},
      year={2020},
      publisher={Frontiers Media SA}
    }


### Environment and Dependencies
Requirements:
* Python 3.7
* Pytorch 0.4.1
* scipy
* scikit-image
* opencv-python
* denseCRF3D

Our code has been tested with Python 3.7, Pytorch 0.4.1, CUDA 10.0 on Ubuntu 18.04.


### Dataset Setup
    ../
    Data/
    ├── subtomograms_snrINF              
    │   ├── cnt.npy              # 22x1 array -- contains the number of subtomograms for each classes (22 classes in total)
    │   ├── data.npy             # 22x1000x32x32x32 array -- contains 22 classes' subtomograms with size of 32x32x32. 1000 subtomogram storage place give, and is indexed by cnt.npy
    │   ├── data_seg.npy         # 22x1000x32x32x32 array -- contains 22 classes' subtomogram segmentation with size of 32x32x32, corresponding with data.npy
    │   ├── testtom_vol_00.npy   # 1xcountsx32x32x32 array -- contains 1st class's subtomograms with size of 32x32x32, counts means number of subtomogram in this class.
    │   ├── testtom_seg_00.npy   # 1xcountsx32x32x32 array -- contains 1st class's subtomogram segmentation with size of 32x32x32, counts means number of subtomogram in this class.
    │   ├── testtom_vol_01.npy  
    │   ├── testtom_seg_01.npy 
    │   ├── testtom_vol_02.npy  
    │   ├── testtom_seg_02.npy  
    │   ├── testtom_vol_03.npy  
    │   ├── testtom_seg_03.npy  
    │   ├── testtom_vol_04.npy  
    │   ├── testtom_seg_04.npy 
    │   ├── testtom_vol_05.npy  
    │   ├── testtom_seg_05.npy  
    │   ├── testtom_vol_06.npy  
    │   ├── testtom_seg_06.npy 
    │   ├── testtom_vol_07.npy  
    │   ├── testtom_seg_07.npy  
    │   ├── testtom_vol_08.npy  
    │   ├── testtom_seg_08.npy  
    │   ├── testtom_vol_09.npy  
    │   ├── testtom_seg_09.npy 
    │   ├── testtom_vol_10.npy  
    │   ├── testtom_seg_10.npy 
    │   ├── testtom_vol_11.npy  
    │   ├── testtom_seg_11.npy  
    │   ├── testtom_vol_12.npy  
    │   ├── testtom_seg_12.npy 
    │   ├── testtom_vol_13.npy  
    │   ├── testtom_seg_13.npy  
    │   ├── testtom_vol_14.npy  
    │   ├── testtom_seg_14.npy  
    │   ├── testtom_vol_15.npy  
    │   ├── testtom_seg_15.npy 
    │   ├── testtom_vol_16.npy  
    │   ├── testtom_seg_16.npy  
    │   ├── testtom_vol_17.npy  
    │   ├── testtom_seg_17.npy 
    │   ├── testtom_vol_18.npy  
    │   ├── testtom_seg_18.npy  
    │   ├── testtom_vol_19.npy  
    │   ├── testtom_seg_19.npy  
    │   ├── testtom_vol_20.npy  
    │   ├── testtom_seg_20.npy 
    │   ├── testtom_vol_21.npy  
    │   └── testtom_seg_21.npy 
    │
    ├── subtomograms_snr10000
    │   ├── cnt.npy
    │   └── ... 
    └── 

Please refer to the above data directory setup to prepare you data, and please read the comments for the file contents.
The training process use cnt.py / data.npy / data_seg.npy. Our dataloader will split the first 1-14 classes as training class and 15-22 classes as testing class.
The testing process use testtom_vol_nn.npy

### To Run Our Code
- Train the model
```bash
python train.py --experiment_name 'train_DuDoRN_R4_pT1' --data_root './Data/' --dataset 'Cartesian' --netG 'DRDN' --n_recurrent 4 --use_prior --protocol_ref 'T1' --protocol_tag 'T2'
```
where \
`--experiment_name` provides the experiment name for the current run, and save all the corresponding results under the experiment_name's folder. \
`--data_root`  provides the data folder directory (with structure illustrated above). \
`--n_recurrent` defines number of recurrent blocks in the DuDoRNet. \
`--protocol_tag` defines target modality to be reconstruct, e.g. T2 or FLAIR. \
`--protocol_ref` defines modality to be used as prior, e.g. T1. \
`--use_prior` defines whether to use prior as indicated by protocol_ref. \
Other hyperparameters can be adjusted in the code as well.

- Test the model
```bash
python test.py --experiment_name 'test_DuDoRN_R4_pT1' --accelerations 5 --resume './outputs/train_DuDoRN_R4_pT1/checkpoints/model_259.pt' --data_root './Data/' --dataset 'Cartesian' --netG 'DRDN' --n_recurrent 4 --use_prior --protocol_ref 'T1' --protocol_tag 'T2'
```
where \
`--accelerations` defines the acceleration factor, e.g. 5 for 5 fold accelerations. \
`--resume` defines which checkpoint for testing and evaluation. \
The test will output an eval.mat containing model's input, reconstruction prediction, and ground-truth for evaluation.

Sample training/test scripts are provided under './scripts/' and can be directly executed.


### Contact 
If you have any question, please file an issue or contact the author:
```
Bo Zhou: bo.zhou@yale.edu
```