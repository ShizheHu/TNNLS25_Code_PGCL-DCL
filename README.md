# TNNLS25_Deep-Multi-view-Clustering-PGCL-DCL

The code and dataset are about the paper "Deep Multi-view Clustering by Pseudo-label Guided Contrastive Learning and Dual Correlation Learning", and all the details are in the ZIP file.

If you are going to using our code, please cite our paper with the following:


@ARTICLE{TNNLS2025DMVC,\
  author={Hu, Shizhe and Zhang, Chengkun and Zou, Guoliang and Lou, Zhengzheng and Ye, Yangdong},\
  journal={IEEE Transactions on Neural Networks and Learning Systems (TNNLS)}, \
  title={Deep Multiview Clustering by Pseudo-Label Guided Contrastive Learning and Dual Correlation Learning}, \
  year={2025},\
  volume={36},\
  number={2},\
  pages={3646-3658},\
  }

More details on the implementation:

This repository provides the implementations of Deep Multi-view Clustering by Pseudo-label Guided Contrastive Learning and Dual Correlation Learning, presented in the paper:


## Installation
Requires Python >= 3.7 (tested on 3.8)

To install the required packages, run:
```
pip install -r requirements.txt
```
from the root directory of the repository. Anaconda is recommended.

## Datasets
### Included dataset
The following datasets are included as files in this project:

- `CCV` (CCV)
## Experiment configuration
Experiment configs are nested configuration objects, where the top-level config is an instance of 
`config.defaults.Experiment`. 

The configuration object for the contrastive model on CCV, for instance, looks like this:
```Python
CCV = Experiment(
    dataset_config=Dataset(name="ccv"),
    model_config=SMVCDIM(
        backbone_configs=(
            MLP(input_size=(5000,)),
            MLP(input_size=(5000,)),
            MLP(input_size=(4000,)),
        ),
        fusion_config=Fusion(method="weighted_mean", n_views=3),
        projector_config=None,
        cm_config=DDC(n_clusters=20),
        loss_config=Loss(
            funcs="ddc_1|ddc_2|ddc_3|Alignment|Clu|mi_fused",
            delta=20.0
        ),
        optimizer_config=Optimizer(scheduler_step_size=50, scheduler_gamma=0.1)
    ),
)
```

## Running an experiment
In the `src` directory, run:
```
python -m models.train -c <config_name> 
```
where `<config_name>` is the name of an experiment config from one of the files in `src/config/experiments/` or from 
'src/config/eamc/experiments.py' (for EAMC experiments).

### Overriding config parameters at the command-line
The default configuration for the experiment, such as `batch_size`, the number of training epochs, and the configuration for DDC, is stored in `defaults.py`.
## Evaluating an experiment
Run the evaluation script:
```Bash
python -m models.evaluate -c <config_name> \ # Name of the experiment config
                          -t <tag> \         # The unique 8-character ID assigned to the experiment when calling models.train
                          --plot             # Optional flag to plot the representations before and after fusion.
```
