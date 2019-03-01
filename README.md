# qebrain / "Bilingual Expert" Can Find Translation Errors

This repository is forked from https://github.com/lovecambi/qebrain, which is a implementation of paper ["Bilingual Expert" Can Find Translation Errors](https://arxiv.org/abs/1807.09433). 

//Since the implementation details, data preprocessing, and other possibilities, it is not guaranteed to reproduce the results in [WMT 2018 QE task](http://www.statmt.org/wmt18/quality-estimation-task.html#results).

After testing on WMT2018 sentencel-level QE De-En task, the code can reproduce the result of in [WMT 2018 QE task](http://www.statmt.org/wmt18/quality-estimation-task.html#results). Note that just using single GPU(p100) is enough for training expert model and estimation model.

## Requirements
1. TensorFlow 1.12 `pip install tensorflow-gpu` ( Noted that it should be run under CUDA9.0 and cudnn7.3.1 )
2. OpenNMT-tf 1.15 `pip install OpenNMT-tf`
We used the following OpenNMT-tf APIs, so the latest OpenNMT-tf may also work if they are not changed. OpenNMT-tf also claimed backward compatibility guarantees.
    * `encoders.self_attention_encoder.SelfAttentionEncoder`
    * `layers.position.SinusoidalPositionEncoder`
    * `decoders.self_attention_decoder.SelfAttentionDecoder`
    * `utils.losses.cross_entropy_sequence_loss`
    * `encoders.BidirectionalRNNEncoder`
    * `layers.ConcatReducer`

## Basic Usage
1. Download the [parallel datasets](http://www.statmt.org/wmt18/translation-task.html#download) from WMT website.
2. Preprocessing data:
      
      tokenization and lowercasing by using tool in moses  https://github.com/moses-smt/mosesdecoder/tree/master/scripts/tokenizer; 
      buliding vocabulary files by running build_vocab.py 
3. The parallel data should be put into foler `data/para` (four emtpty files for representative purpose), and the example vocab files are in folder `data/vocab`.
4. Run `./expert_train.sh` to train bilingual expert model, and due to the large dataset, we provide the multi GPU implementation.
5. Download the [QE dataset](https://lindat.mff.cuni.cz/repository/xmlui/handle/11372/LRT-2619). An example dataset of sentence level De-En QE task has been downloaded and preprocessed in folder `data/qe`, including human features (If no human feature is prepared, set the argument `--use_hf=False`). 
6. Run `./qe_train.sh` to train the quality estimation model, and due to the small dataset, we only provide the single GPU implementation.
7. Run `./qe_infer.sh` to make the inference on dataset without labels.

## Citation
If you use this code for your research, please cite our papers.
```
@article{fan2018bilingual,
  title={" Bilingual Expert" Can Find Translation Errors},
  author={Fan, Kai and Li, Bo and Zhou, Fengming and Wang, Jiayi},
  journal={arXiv preprint arXiv:1807.09433},
  year={2018}
}

@inproceedings{wang2018alibaba,
  title={Alibaba Submission for WMT18 Quality Estimation Task},
  author={Wang, Jiayi and Fan, Kai and Li, Bo and Zhou, Fengming and Chen, Boxing and Shi, Yangbin and Si, Luo},
  booktitle={Proceedings of the Third Conference on Machine Translation: Shared Task Papers},
  pages={809--815},
  year={2018}
}
```

## Reference
Some of the utility functions are referred from [TensorFlow NMT](https://github.com/tensorflow/nmt).
