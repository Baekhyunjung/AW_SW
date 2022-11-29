# Moving Object Segmentation in 3D LiDAR Data: A Learning-based Approach Exploiting Sequential Data

It is a manual of code implementing so that you can use the open source of the paper conveniently.

<br/>

### Go into GitHub and download the source code.
Original GitHub link: <https://github.com/PRBonn/LiDAR-MOS>

Proceed git clone or use git bash command.
```
git clone https://github.com/PRBonn/LiDAR-MOS.git
```
<br/>

### Prepare training data
Generate the residual images.
```
python3 utils/gen_residual_images.py
```

Download toy dataset.

If you want to test every step quickly, we recommend downloading the toy dataset [here](https://www.ipb.uni-bonn.de/html/projects/LiDAR-MOS/LiDAR_MOS_toy_dataset.zip). It has a smaller size than the original dataset.

<br/>

### Inferring
Download pretrained model.

Based on this paper, we conduct an inference using SalaNext, which has higher accuracy.
Download pretrained model with one residual image [here](https://www.ipb.uni-bonn.de/html/projects/LiDAR-MOS/model_salsanext_residual_1.zip).

Move the path to the location of the infer.py code.
```
cd mos_SalsaNext/train/tasks/semantic
```
Start the inference.
```
python3 infer.py -d ./data -m ./data/model_salsanext_residual_1 -l ./data/predictions_salsanext_residual_1_new -s valid
```
Option : **-d** is dataset path, **-m** is pretrained model path, **-l** is new predictions ouput path.

<br/>

### Training
Download [KITTI-Odometry dataset](https://www.cvlibs.net/datasets/kitti/eval_odometry.php) and [Semantic-Kitti dataset](http://semantic-kitti.org/dataset.html).

Move the path to the location of the ./train.sh.
```
cd mos_SalsaNext/train/tasks/semantic
```

Start the training.
```
./train.sh -d path/to/kitti/dataset -a salsanext_mos.yml -l path/to/log -c 0
```
<br/>

### Evaluation
We call the moving (dynamic) status as **D** and the static status as **S**.
|0|dynamic|static|
|---|---|---|
|dynamic|TD|FS|
|static|FD|TS|

<br/>
Evaluation metric is IoU.
<br/>

$$ IoU_{MOS} = \frac{TD}{TD+FD+FS} $$
<br/>
```
python utils/evaluate_mos.py -d data -p data/predictions_salsanext_residual_1_valid -s valid
```
<br/>

### Visualization
```
python3 utils/visualize_mos.py -d data -p data/predictions_salsanext_residual_1_valid -s 8
```
Option : **-d** is dataset path, **-p** is prediction path, **-s** is sequence number.(8 = validation set)
Navigation : **n** is next scan, **b** is previous scan, **esc** or **q** exits.
