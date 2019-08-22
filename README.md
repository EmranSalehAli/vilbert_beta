# ViLBERT <img src="fig/vilbert_trim.png" width="45">

Code and pre-trained models for **ViLBERT: Pretraining Task-Agnostic VisiolinguisticRepresentations for Vision-and-Language Tasks**.



*Note: This is beta release which * 


## Repository Setup

1. Create a fresh conda environment, and install all dependencies.

```text
conda create -n vilbert python=3.6
conda activate vilbert
git clone https://github.com/jiasenlu/vilbert_v0.1
cd vilbert_v0.1
pip install -r requirements.txt
```

2. Install pytorch
```
conda install pytorch torchvision cudatoolkit=10.0 -c pytorch
```

3. Install apx, follows https://github.com/NVIDIA/apex

4. Install this codebase as a package in this environment.
```text
python setup.py develop
```

## Data Setup

Check `README.md` under `data` for more details.  


## Pre-trained model for Evaluation

| Model | Objective | Link |
|:-------:|:------:|:------:|
|ViLBERT 6-Layer| Conceptual Caption |[Link]()|
|ViLBERT 6-Layer| VQA |[Link]()|
|ViLBERT 6-Layer| VCR |[Link]()|
|ViLBERT 6-Layer| RefCOCO+ |[Link]()|
|ViLBERT 6-Layer| Image Retrieval |[Link]()|

### Zero-Shot Image Retrieval

We can directly use the Pre-trained ViLBERT model for zero-shot image retrieval tasks on Flickr30k. 

1: Download the pretrained model with objective `Conceptual Caption` and put it under `save`

2: Update `featyres_h5path1` and `val_annotations_jsonpath` in  `vlbert_task.yml` to load the Flickr30k testset image feature and jsonfile (defualt is training feature). 

3: Use the following command to evaluate pre-trained 6 layer ViLBERT model. (only support single GPU for evaluation now):

```bash
python eval_retrieval.py --bert_model bert-base-uncased --from_pretrained save/bert_base_6_layer_6_connect/pytorch_model_9.bin --config_file config/bert_base_6layer_6conect.json --task 3 --split test --batch_size 1 --zero_shot
```

### Image Retrieval

1: Download the pretrained model with objective `Image Retrieval` and put it under `save`

2: Update `featyres_h5path1` and `val_annotations_jsonpath` in  `vlbert_task.yml` to load the Flickr30k testset image feature and jsonfile (defualt is training feature). 

3: Use the following command to evaluate pre-trained 6 layer ViLBERT model. (only support single GPU for evaluation now):

```bash
python eval_retrieval.py --bert_model bert-base-uncased --from_pretrained save/RetrievalFlickr30k_bert_base_6layer_6conect-pretrained/pytorch_model_19.bin --config_file config/bert_base_6layer_6conect.json --task 3 --split test --batch_size 1
```

### VQA

1: Download the pretrained model with objective `VQA` and put it under `save`

2: To test on held out validation split, use the following command: 

```

```

### VCR

1: Download the pretrained model with objective `VCR` and put it under `save`

2: To test on VCR Q->A

```

```

3: To test on VCR QA->R

```

```

### RefCOCO+

1: Download the pretrained model with objective `RefCOCO+` and put it under `save`

2: We use the Pre-computed detections/masks from [MAttNet](https://github.com/lichengunc/MAttNet) for fully-automatic comprehension task, Check the MAttNet repository for more details. 

3: To test on the RefCOCO+ val set and use the following command:

```bash
python eval_tasks.py --bert_model bert-base-uncased --from_pretrained save/refcoco+_bert_base_6layer_6conect-pretrained/pytorch_model_19.bin --config_file config/bert_base_6layer_6conect.json --task 4
```

## Visiolinguistic Pre-training

Once you extracted all the image features, to train the model: 

```

```

train the model in a distributed setting:

```

```



## TASKS

### VQA 

To fintune a 6-layer ViLBERT model for VQA with 8 GPU. `--tasks 1` means VQA tasks. Check `vlbert_tasks.yml` for more settings for VQA tasks.  

```bash
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=1 --node_rank=0 train_tasks.py --bert_model bert-base-uncased --from_pretrained save/bert_base_6_layer_6_connect_freeze_0/pytorch_model_8.bin  --config_file config/bert_base_6layer_6conect.json  --learning_rate 4e-5 --num_workers 16 --tasks 1 --save_name pretrained
```

### VCR

Similarly, to finetune a 6-layer vilbert model for VCR task, run the following commands. Here we joint train `Q->A ` and `QA->R` tasks, so the tasks is specified as `--tasks 6-7`

```
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=1 --node_rank=0 train_tasks.py --bert_model bert-base-uncased --from_pretrained save/bert_base_6_layer_6_connect_freeze_0/pytorch_model_8.bin  --config_file config/bert_base_6layer_6conect.json  --learning_rate 2e-5 --num_workers 16 --tasks 6-7 --save_name pretrained
```

### Refer Expression
```
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=1 --node_rank=0 train_tasks.py --bert_model bert-base-uncased --from_pretrained save/bert_base_6_layer_6_connect_freeze_0/pytorch_model_8.bin  --config_file config/bert_base_6layer_6conect.json  --learning_rate 4e-5 --num_workers 16 --tasks 11 --save_name pretrained
```

### Image Retrieval
```bash
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=1 --node_rank=0 train_tasks.py --bert_model bert-base-uncased --from_pretrained save/bert_base_6_layer_6_connect_freeze_0/pytorch_model_8.bin  --config_file config/bert_base_6layer_6conect.json  --learning_rate 4e-5 --num_workers 9 --tasks 11 --save_name pretrained
```

### Add your own tasks
```

```
## Why does ViLBERT look like <img src="fig/vilbert_trim.png" width="45">? 

<p align="center">
<img src="fig/vilbert.png" width="400" >
</p>