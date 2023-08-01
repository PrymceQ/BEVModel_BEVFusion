# BEVModel_BEVFusion🚖
The reproduction project of the BEV model # BEVFusion, which includes some code annotation work.

Thanks for the BEVFusion authors！[Paper](https://arxiv.org/abs/2205.13542) | [Code](https://github.com/mit-han-lab/bevfusion)

## 🌵Necessary File Format
- configs/
- data/nuscenes/
  - maps/
  - samples/
  - sweeps/
  - v1.0-test/
  - v1.0-trainval/
- mmdet3d/
- tools/
- pretrained/

## 🌵Build Envs
You can refer to the official configuration environment documentation. [Official Git](https://github.com/mit-han-lab/bevfusion)
> BEVFusion's official introduction document is very comprehensive and detailed.

Or use the Conda env configuration file we provide.
```
conda env create -f bevfusion_env.yaml
```

After that, you should 
```
python setup.py develop
```

## 🌵Data create

```
python tools/create_data.py nuscenes --root-path ./data/nuscenes --out-dir ./data/nuscenes --extra-tag nuscenes
```

## Pretrained ckpts download

```
./tools/download_pretrained.sh
```

## 🌵Train Code
```
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
torchpack dist-run -np 8 python tools/train.py configs/nuscenes/det/transfusion/secfpn/camera+lidar/swint_v0p075/convfuser.yaml --model.encoders.camera.backbone.init_cfg.checkpoint pretrained/swint-nuimages-pretrained.pth --load_from pretrained/lidar-only-det.pth 
```

## 🌵Test Code
```
torchpack dist-run -np 8 python tools/test.py configs/nuscenes/det/transfusion/secfpn/camera+lidar/swint_v0p075/convfuser.yaml pretrained/bevfusion-det.pth --eval bbox
```

## 🌵Training Result Record

ID | Name | mAP | NDS | mATE | mASE | mAOE | mAVE | mAAE | Epochs | Data | Batch_size | GPUs | Train_time | Eval_time | Log_file
:----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :----------- | :-----------
0 | bevfusion cam+lidar | 0.6807 | 0.7107 | 0.2871 | 0.2531 | 0.3091 | 0.2632 | 0.1844 |  6 | All | 16, sample per gpu=2 | 8 x Nvidia Geforce 3090 | 12hours | 71.1s | runs/bevfusion_res_log/


## 🌵Key Model Files

Here we have made simple annotations on some key model files in Chinese, these annotations are based on "configs/nuscenes/det/transfusion/secfpn/camera+lidar/swint_v0p075/convfuser.yaml" config. 

You can find them in:
- mmdet3d\models\fusion_models\bevfusion.py
