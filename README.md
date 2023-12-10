# MolFilterGAN



**MolFilterGAN: A Progressively Augmented Generative Adversarial Network for Triaging AI-designed Molecules**

Published on [Journal of Cheminformatics](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-023-00711-1).

## Overview
Artificial intelligence (AI)-based molecular design methods, especially deep generative models for generating novel molecule structures, have gratified our imagination to explore unknown chemical space without relying on brute-force exploration. However, whether designed by AI or human experts, the molecules need to be accessibly synthesized and biologically evaluated, and the trial-and-error process remains a resources-intensive endeavor. Therefore, AI-based drug design methods face a major challenge of how to prioritize the molecular structures with potential for subsequent drug development. This study indicates that common filtering approaches based on traditional screening metrics fail to differentiate AI-designed molecules. To address this issue, we propose a novel molecular filtering method, MolFilterGAN, based on a progressively augmented generative adversarial network. Comparative analysis shows that MolFilterGAN outperforms conventional screening approaches based on drug-likeness or synthetic ability metrics. Retrospective analysis of AI-designed discoidin domain receptor 1 (DDR1) inhibitors shows that MolFilterGAN significantly increases the efficiency of molecular triaging. Further evaluation of MolFilterGAN on eight external ligand sets suggests that MolFilterGAN is useful in triaging or enriching bioactive compounds across a wide range of target types. These results highlighted the importance of MolFilterGAN in evaluating molecules integrally and further accelerating molecular discovery especially combined with advanced AI generative models.

<img src="https://github.com/myzhengSIMM/MolFilterGAN/blob/main/workflow_MolFilterGAN.png" alt="MolFilterGAN Workflow">


## Requirement
```git clone https://github.com/myzhengSIMM/MolFilterGAN``` 

```cd MolFilterGAN``` 

This project requires the following libraries.

- NumPy
- Pandas
- PyTorch > 1.2
- RDKit
- tensorboardX
## Data and Trained_Models (Google_Drive)

All Data used for Training or Evaluating and the Trained_Models are available at:

https://drive.google.com/drive/folders/1uN7a5m1PmhcXfs5OuOXWPbxyF_KKuZ3A?usp=sharing

`Datasets.zip` contains all datasets for training  MolFilterGAN

`BenchmarkDatasets.zip` contains all the benchmark dataset for evaluating metrics.

`pretrained_G.ckpt` is a pre-trained initial generator

`pretrained_D.ckpt` is a pre-trained initial discriminator

`ADtrained_D.ckpt` is an adversarial-trained discriminator

After Downloading, you can simply unzip the```.zip``` files to get ```Datsets/``` ,  ``` BenchmarkDatasets/``` and  ```  PCBA/```,

and create the directions by ```mkdir AD_save  pretrainD_save  pretrainG_save``` then put the ```.ckpt``` files in the corresponding directions.

**Finally the folder structure will look like this**: 

```bash
MolFilterGAN
|___AD_save 
|   |___ADtrained_D.ckpt 			# an adversarial-trained discriminator
| 
|___BenchmarkDatasetse 				# contains all the benchmark dataset for evaluating metrics.
|   |chembl-sample10000.smi
|   |___...
| 		
|___Datasets						# contains all datasets for training  MolFilterGAN
|   |Data4InitD_neg.smi
|   |___...
| 
|___PCBA
|   |ALDH1_active_T_rd_rm_less.smi
|   |___...
| 
|___pretrainD_save
|   |___pretrained_D.ckpt			# a pre-trained initial discriminator
|
|___pretrainG_save
|   |___pretrained_G.ckpt			# a pre-trained initial generator
| 
|___results							
|   |___.csv
|   |___...
| 
|___AdversarialTraining.py
| 
|___Dataset.py
...
```

## Training a initial generator

`python PretrainG.py --infile_path Datasets/Data4InitG.smi --log_path test_init_G_log --model_save_path test_init_G_save`

## Training a initial discriminator

`python PretrainD.py --infile_path Datasets/Data4InitD.txt --log_path test_init_D_log --model_save_path test_init_D_save`

## Adversarial Training

`python AdversarialTraining.py --infile_path Datasets/Data4InitD.txt --log_path test_AD_log --model_save_path test_AD_save --load_dir_G pretrainG_save/pretrained_G.ckpt --load_dir_D pretrainD_save/pretrained_D.ckpt`

## Prediction 

You can easily use the trained_discrimination_models by changing the ```infile_path``` and the ```load_dir``` like: 

`python Prediction.py --infile_path './BenchmarkDatasets/GA-sample10000.smi'  --load_dir AD_save/ADtrained_D.ckpt`
