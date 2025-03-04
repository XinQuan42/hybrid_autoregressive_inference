# Hybrid Autoregressive Inference for Scalable Multi-hop Explanation Regeneration (AAAI 2022 - Main Track)

Regenerating natural language explanations in the scientific domain has been proposed as a benchmark to evaluate complex multi-hop and explainable inference. In this context, large language models can achieve state-of-the-art performance when employed as cross-encoder architectures and fine-tuned on human-annotated explanations. 
However, while much attention has been devoted to the quality of the explanations, the problem of performing inference efficiently is largely under-studied. Cross-encoders, in fact, are intrinsically not scalable, possessing limited applicability to real-world scenarios that require inference on massive facts banks.

To enable complex multi-hop reasoning at scale, this paper focuses on bi-encoder architectures, investigating the problem of scientific explanation regeneration at the intersection of dense and sparse models. Specifically, we present SCAR (for Scalable Autoregressive Inference), a hybrid framework that iteratively combines a Transformer-based bi-encoder with a sparse model of explanatory power, designed to leverage explicit inference patterns in the explanations. 

Our experiments demonstrate that the hybrid framework significantly outperforms previous sparse models, achieving performance comparable with that of state-of-the-art cross-encoders while being approx 50 times faster and scalable to corpora of millions of facts. Further analyses on semantic drift and multi-hop question answering reveal that the proposed hybridisation boosts the quality of the most challenging explanations, contributing to improved performance on downstream inference tasks.

![Image description](approach.png)

# Reproducibility

Welcome! :) 

In this repository, you can find the code (`explanation_regeneration_experiment.py`) to reproduce the results obtained by [SCAR](https://arxiv.org/abs/2107.11879) (Scalable Autoregressive Inference) on the [WorldTree Multi-hop Explanation Regeneration Task](https://github.com/umanlp/tg2019task).

## Setup:

Install the [sentence-transformers](https://www.sbert.net/) package:

`pip install -U sentence-transformers`

Install the [faiss-gpu](https://pypi.org/project/faiss-gpu/) package:

`pip install faiss-gpu`

## Dense Encoder:

The pre-trained Sentence-BERT bi-encoder used in our experiments can be downloaded [here!](https://drive.google.com/file/d/1iz38q8EIYZdO9U7mAMVz1qUprU8jmEwI/view?usp=sharing)

To reproduce our experiments, download the model and store it in `./models`.

**Training:**

If you want to train the dense encoder from scratch, you can use the released `training.py` script. This will create a new [Sentence-BERT](https://www.sbert.net/) model (`bert-base-uncased`) and fine-tune it on the inference chains stored in `./data/training/chains_train.csv` via contrastive loss.

If needed, you can regenerate the training-set using the `extract_chains.py` script.

## Multi-hop Explanation Regeneration Experiment:

Once the dense model is downloaded and unzipped in the apposite folder, run the following command to start the experiment:

`python ./explanation_regeneration_experiment.py`

This will create the [FAISS](https://faiss.ai/) index and perform multi-hop inference using SCAR.

**Compute the Mean Average Precision (MAP) score:** 

Once the experiment is completed, you can compute the Mean Average Precision (MAP) using the following command:

`./evaluate.py --gold=./data/questions/dev.tsv prediction.txt`

The experiment is performed by default on the dev-set. If you want to reproduce our results on the test-set, you can update the test dataset and hypotheses in `explanation_regeneration_experiment.py` and submit the final `prediction.txt` to the official [leaderboard](https://competitions.codalab.org/competitions/20150#results).

## Citation
We hope you find this repository useful. If you use SCAR in your work, please consider citing our paper!

```
@article{valentino2022hybrid, 
  title={Hybrid Autoregressive Inference for Scalable Multi-Hop Explanation Regeneration},
  author={Valentino, Marco and Thayaparan, Mokanarangan and Ferreira, Deborah and Freitas, Andr{\'e}},
  journal={Proceedings of the AAAI Conference on Artificial Intelligence},
  volume={36},
  number={10}, 
  url={https://ojs.aaai.org/index.php/AAAI/article/view/21392}, 
  DOI={10.1609/aaai.v36i10.21392},
  year={2022},
  month={Jun.}, 
  pages={11403-11411} 
}
```

For any issues or questions, feel free to contact us at marco.valentino@idiap.ch
