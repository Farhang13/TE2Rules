# TE2Rules
[![License](https://img.shields.io/badge/license-BSD-green.svg)](https://github.com/groshanlal/TE2Rules/blob/master/LICENSE)
[![Paper](http://img.shields.io/badge/cs.LG-arXiv%3A2206.14359-orange.svg)](https://arxiv.org/abs/2206.14359)
[![PyPI](https://img.shields.io/pypi/v/te2rules?color=blue)](https://pypi.org/project/te2rules/)
[![PyPI](https://img.shields.io/readthedocs/te2rules)](https://te2rules.readthedocs.io/en/latest/index.html)



TE2Rules is a technique to explain Tree Ensemble models (TE) like XGBoost, Random Forest, trained on a binary classification task, using a rule list. The extracted rule list (RL) captures the necessary and sufficient conditions for classification by the Tree Ensemble. The algorithm used by TE2Rules is based on Apriori Rule Mining. For more details on the algorithm, please check out our [paper](https://arxiv.org/abs/2206.14359).

TE2Rules provides a ```ModelExplainer``` which takes a trained TE model and training data to extract rules. The training data is used for extracting rules with relevant combination of input features. Without data, an explainer would have to extract rules for all possible combinations of input features, including those combinations which are extremely rare in the data. 


## Installation:
TE2Rules package is available on PyPI and can be installed with pip:
```
pip install te2rules
```

## Documentation

The official documentation of TE2Rules can be found at [te2rules.readthedocs.io](https://te2rules.readthedocs.io/). 

TE2Rules contains a ```ModelExplainer``` class with an ```explain()``` method which returns a rule list corresponding to the positive class prediciton of the tree ensemble. While using the rule list, any data instance that does not trigger any of the extracted rules is to be interpreted as belonging to the negative class. The ```explain()``` method has two tunable parameters to control the interpretability, faithfulness, runtime and coverage of the extracted rules. These are: 
- ```min_precision```: ```min_precision``` controls the minimum precision of extracted rules. Setting it to a smaller threhsold, allows extracting shorter (more interpretable, but less faithful) rules. By default, the algorithm uses a minimum precision threshold of 0.95.  
- ```num_stages```: The algorithm runs in stages starting from stage 1, stage 2 to all the way till stage n where n is the number of trees in the ensemble. Stopping the algorithm at an early stage  results in a few short rules (with quicker run time, but less coverage in data). By default, the algorithm explores all stages before terminating.

For evaluating the performance of the extracted rule list, the ```ModelExplainer``` provides a method ```get_fidelity()``` which returns the fractions of data for which the rule list agrees with the tree ensemble. ```get_fidelity()``` returns the fidelity on positives, negatives and overall fidelity. 

## Usage

The following notebook shows a typical use case of TE2Rules on Adult Income Data. The notebook can be found [here](https://github.com/groshanlal/TE2Rules/blob/master/notebooks/demo-adult-income.ipynb). Let us start with importing ```te2rules``` and other relevant libraries
![TE2Rules Adult Screenshot1](https://raw.githubusercontent.com/groshanlal/TE2Rules/master/docs/images/1-intro.png)

Let us load the training and testing data. All the data used in this notebook are preprocessed already and can be found [here](https://github.com/groshanlal/TE2Rules/tree/master/data). The data can also be generated by running ```python3 data_prep/data_prep_adult.py```.
![TE2Rules Adult Screenshot2](https://raw.githubusercontent.com/groshanlal/TE2Rules/master/docs/images/2-data.png)

The tree ensemble model used in this notebook is a XGBoost model with 10 trees.
![TE2Rules Adult Screenshot3](https://raw.githubusercontent.com/groshanlal/TE2Rules/master/docs/images/3-train.png)

Let us use TE2Rules ```ModelExplainer``` to explain the positive class prediciton by the XGBoost model. We observe that TE2Rules extracts 5 rules to explain more than 99% of the positive class prediction by the tree ensemble model. In this usgae, we use the default values of ```min_precision``` (0.95) and  ```num_stages``` (10), since the algorithm runs quickly for a tree ensemble with 10 trees.
![TE2Rules Adult Screenshot4](https://raw.githubusercontent.com/groshanlal/TE2Rules/master/docs/images/4-explain.png)
![TE2Rules Adult Screenshot5](https://raw.githubusercontent.com/groshanlal/TE2Rules/master/docs/images/5-evaluate.png)

## For reproducing results in the paper :
Run the follwing python scripts to generate the results in the [paper](https://arxiv.org/abs/2206.14359):
```
python3 global_baseline_experiment.py
python3 global_te2rules_experiment.py
python3 outcome_explanation_experiments.py
``` 

## License
BSD 2-Clause License, see [LICENSE](https://github.com/groshanlal/TE2Rules/blob/master/LICENSE).

## Citation
Please cite [TE2Rules](https://arxiv.org/abs/2206.14359) in your publications if it helps your research:
```
@article{te2rules2022,
  title={TE2Rules: Extracting Rule Lists from Tree Ensembles},
  author={Lal, G Roshan and Chen, Xiaotong and Mithal, Varun},
  journal={arXiv preprint arXiv:2206.14359},
  year={2022}
}
```
