# SG_PR

Code for IROS2020 paper [Semantic Graph Based Place Recognition for 3D Point Clouds](https://ras.papercept.net/proceedings/IROS20/0170.pdf)

![](./doc/pipeline.png)

Pipeline overview.

## Citation

If you think this work is useful for your research, please consider citing:

```
@inproceedings{kong2020semantic,
  title={Semantic Graph based Place Recognition for Point Clouds},
  author={Kong, Xin and Yang, Xuemeng and Zhai, Guangyao and Zhao, Xiangrui and Zeng, Xianfang and Wang, Mengmeng and Liu, Yong and Li, Wanlong and Wen, Feng},
  booktitle={2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={8216--8223},
  year={2020},
  organization={IEEE}
}
```

## Requirements
We recommend python3.6. You can install required dependencies by:
```bash
pip install -r requirements.txt
```

## Training

### Data structure

The data structure is:

```bash
data
    |---00
    |    |---000000.json   
    |    |---000001.json
    |    |---...
    |
    |---01
    |    |---000000.json
    |    |---000001.json
    |    |---...
    |
    |---...
    |
    |---00.txt
    |---01.txt
    |---...
```
You can download the provided [preprocessed data](https://drive.google.com/file/d/1eu4G008gvAJGjU-M8qBvTN0JLHG2B-OB/view?usp=sharing).

### Configuration file

Before training the model, you need to modify the configuration file in ./config according to your needs. The main parameters are as follows:
- graph_pairs_dir: the root dir of your dataset.
- batch_size: batch size.
- <span id="p_thresh">p_thresh</span>: distance threshold for positive samples. If the distance between two samples is less than p_thresh meters, they will be treated as positive samples. The distance threshold for negative samples is set to 20 meters by default. Note that your training sample pairs should not contain samples with a distance greater than p_thresh meters and less than 20 meters.
- model: pre-trained model, if you want to train a completely new model from scratch, you should set it to empty.
- train_sequences: list of training data.
- eval_sequences: list of validation data.
- logdir: path to save training results.

### Training model

After preparing the data and modifying the configuration file, you can start training. Just run:

```bash
python main_sg.py
```

## Testing

### eval_pair

This example takes a pair of graphs as input and output their similarity score. To run this example, you need to set the following parameters:
- model: the model file.
- pair_file: a pair of graph.

Then just run:
```bash
python eval_pair.py
```

### eval_batch

This example tests a sequence, the results are it's PR curve and F1 max score. To run this example, you need to set the following parameters:
- model: the model file.
- batch_size: batch size.
- p_thresh: e.g., 3m, 5m, 10m.
- graph_pairs_dir: the path which contains test sequences.
- sequences: list of test sequences.
- output_path: Path to save test results.
- show: Whether to display the pr curve in real time.

Then just run:
```bash
python eval_batch.py
```

<!--
Todo
## Other methods
#### Scan Context
##### Ford
1. Generate feature database by 'gen_SC_db_ford.py'
2. Compute distance and plot PR curve by 'eval_SC_list_Ford.py'


#### M2DP
##### Ford
1. Generate feature database by 'evaluate_Ford.m'
2. Compute distance and plot PR curve by 'eval_SC_list_Ford.py'
-->

## TODO
- [ ] Support Ford Campus Dataset
- [ ] Release compared methods e.g., [PointNetVLAD](https://github.com/mikacuy/pointnetvlad) trained on KITTI (PV-KITTI)
- [ ] Release preprocessing code


## Acknowledgement

Thanks to the source code of some great works such as [SIMGNN](https://github.com/benedekrozemberczki/SimGNN), [DGCNN](https://github.com/WangYueFt/dgcnn).