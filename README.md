# COVID-Net Open Source Initiative

**Note: The models provided here are intended to be used as reference models that can be built upon and enhanced as new data becomes available. They are currently at a research stage and not yet intended as production-ready models (not meant for direct clinicial diagnosis), and we are working continuously to improve them as new data becomes available. Please do not use it for self-diagnosis and seek help from your local health authorities.**

<p align="center">
  <img src="assets/covidnet-small-exp.png" alt="photo not available" width="70%" height="70%">
  <br>
  <em>Example chest radiography images of COVID-19 cases from 2 different patients and their associated critical factors (highlighted in red) as identified by GSInquire.</em>
</p>

This repo is an extension of [COVID-Net](https://github.com/lindawangg/COVID-Net), here you can find several data sets, as well as scripts for pre-processing and further training of artificial neural networks.

For a detailed description of the methodology behind COVID-Net and a full description of the COVIDx dataset, please click [here](assets/COVID_Netv2.pdf).

If there are any technical questions, please contact:
* eduardo.perez@imibic.org
* amoya@uco.es

## Requirements

The main requirements are listed below:

* Tested with Tensorflow 1.13 and 1.15
* OpenCV 4.2.0
* Python 3.6
* Numpy
* OpenCV
* Scikit-Learn
* Matplotlib

Additional requirements to generate dataset:

* PyDicom
* Pandas
* Jupyter

## COVIDx Dataset

**Update: we have released the brand-new COVIDx dataset with 16,756 chest radiography images across 13,645 patient cases.**

The current COVIDx dataset is constructed by the following open source chest radiography datasets:
* https://github.com/ieee8023/covid-chestxray-dataset
* https://www.kaggle.com/c/rsna-pneumonia-detection-challenge

We especially thank the Radiological Society of North America and others involved in the RSNA Pneumonia Detection Challenge, and Dr. Joseph Paul Cohen and the team at MILA involved in the COVID-19 image data collection project, for making data available to the global community.

### Steps to generate the dataset

1. Download the datasets listed above
 * `git clone https://github.com/ieee8023/covid-chestxray-dataset.git`
 * go to this [link](https://www.kaggle.com/c/rsna-pneumonia-detection-challenge/data) to download the RSNA pneumonia dataset
2. Create a `data` directory and within the data directory, create a `train` and `test` directory
3. Use [create\_COVIDx\_v2.ipynb](create_COVIDx_v2.ipynb) to combine the two dataset to create COVIDx. Make sure to remember to change the file paths.
4. We provide the train and test txt files with patientId, image path and label (normal, pneumonia or COVID-19). The description for each file is explained below:
 * [train\_COVIDx.txt](train_COVIDx.txt): This file contains the samples used for training.
 * [test\_COVIDx.txt](test_COVIDx.txt): This file contains the samples used for testing.
5. Now you should have 2 dirs (train and test), optionally you can organize the images in folders accordingly classes using [covidx\_setup\_classes.ipynb](notebooks/covidx_setup_classes.ipynb) 

### COVIDx data distribution

Chest radiography images distribution
|  Type | Normal | Pneumonia | COVID-19 | Total |
|:-----:|:------:|:---------:|:--------:|:-----:|
| train |  7966  |    8514   |    66    | 16546 |
|  test |   100  |     100   |    10    |   210 |

Patients distribution
|  Type | Normal | Pneumonia | COVID-19 |  Total |
|:-----:|:------:|:---------:|:--------:|:------:|
| train |  7966  |    5429   |    48    |  13443 |
|  test |   100  |      97   |     5    |    202 |

## Chest X-Ray Images (Pneumonia)

You can download this dataset from kaggle:
* https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia

### Steps to generate the dataset

1. Download the datasets listed above
 * https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia
2. You will see three folders: train, test and val. This folders contain images of two types: NORMAL and PNEUMONIA. 
3. Use  to divide PNEUMONIA folder into two folders: pneumonia-virus and pneumonia-bacteria. It will be done for train, test and val folders. 

### Chest X-Ray Images (Pneumonia) data distribution


## Training and Evaluation
The network takes as input an image of shape (N, 224, 224, 3) and outputs the softmax probabilities as (N, 3), where N is the number of batches.
If using the TF checkpoints, here are some useful tensors:

* input tensor: `input_1:0`
* output tensor: `dense_3/Softmax:0`
* label tensor: `dense_3_target:0`
* class weights tensor: `dense_3_sample_weights:0`
* loss tensor: `loss/mul:0`

### Steps for training
Releasing TF training script from pretrained model soon.
<!--1. To train from scratch, `python train.py`
2. To train from an existing hdf5 file, `python train.py --checkpoint output/example/cp-0.hdf5`
3. For more options and information, `python train.py --help`
4. If you have a GenSynth account, to convert hdf5 file to TF checkpoints,
`python export_to_meta.py --weightspath output/example --weightspath cp-0.hdf5`-->  

### Steps for evaluation

1. We provide you with the tensorflow evaluation script, [eval.py](eval.py)
2. Locate the tensorflow checkpoint files
3. To evaluate a tf checkpoint, `python eval.py --weightspath models/COVID-Netv2 --metaname model.meta_eval --ckptname model-2069`
4. For more options and information, `python eval.py --help`

### Steps for inference
**DISCLAIMER: Do not use this prediction for self-diagnosis. You should check with your local authorities for the latest advice on seeking medical assistance.**

1. Download a model from the [pretrained models section](#pretrained-models)
2. Locate models and xray image to be inferenced
3. To inference, `python inference.py --weightspath models/COVID-Netv2 --metaname model.meta_eval --ckptname model-2069 --imagepath assets/ex-covid.jpeg`
4. For more options and information, `python inference.py --help`

## Results
These are the final results for COVID-Net Small and COVID-Net Large.   

### COVIDNet Small
<p align="center">
  <img src="assets/cm-covidnet-small.png" alt="photo not available" width="50%" height="50%">
  <br>
  <em>Confusion matrix for COVID-Net on the COVIDx test dataset.</em>
</p>

<div class="tg-wrap" align="center"><table class="tg">
  <tr>
    <th class="tg-7btt" colspan="3">Sensitivity (%)</th>
  </tr>
  <tr>
    <td class="tg-7btt">Normal</td>
    <td class="tg-7btt">Pneumonia</td>
    <td class="tg-7btt">COVID-19</td>
  </tr>
  <tr>
    <td class="tg-c3ow">95.0</td>
    <td class="tg-c3ow">91.0</td>
    <td class="tg-c3ow">80.0</td>
  </tr>
</table></div>

<div class="tg-wrap"><table class="tg">
  <tr>
    <th class="tg-7btt" colspan="3">Positive Predictive Value (%)</th>
  </tr>
  <tr>
    <td class="tg-7btt">Normal</td>
    <td class="tg-7btt">Pneumonia</td>
    <td class="tg-7btt">COVID-19</td>
  </tr>
  <tr>
    <td class="tg-c3ow">91.3</td>
    <td class="tg-c3ow">93.8</td>
    <td class="tg-c3ow">88.9</td>
  </tr>
</table></div>

### COVID-Net Large
<p align="center">
  <img src="assets/cm-covidnet-large.png" alt="photo not available" width="50%" height="50%">
  <br>
  <em>Confusion matrix for COVID-Net on the COVIDx test dataset.</em>
</p>

<div class="tg-wrap" align="center"><table class="tg">
  <tr>
    <th class="tg-7btt" colspan="3">Sensitivity (%)</th>
  </tr>
  <tr>
    <td class="tg-7btt">Normal</td>
    <td class="tg-7btt">Pneumonia</td>
    <td class="tg-7btt">COVID-19</td>
  </tr>
  <tr>
    <td class="tg-c3ow">94.0</td>
    <td class="tg-c3ow">90.0</td>
    <td class="tg-c3ow">90.0</td>
  </tr>
</table></div>

<div class="tg-wrap"><table class="tg">
  <tr>
    <th class="tg-7btt" colspan="3">Positive Predictive Value (%)</th>
  </tr>
  <tr>
    <td class="tg-7btt">Normal</td>
    <td class="tg-7btt">Pneumonia</td>
    <td class="tg-7btt">COVID-19</td>
  </tr>
  <tr>
    <td class="tg-c3ow">90.4</td>
    <td class="tg-c3ow">93.8</td>
    <td class="tg-c3ow">90.0</td>
  </tr>
</table></div>

## Pretrained Models

|  Type | COVID-19 Sensitivity | # Params (M) | MACs (G) |        Model        |
|:-----:|:--------------------:|:------------:|:--------:|:-------------------:|
|  ckpt |         80.0         |     116.6    |   2.26   |[COVID-Net Small](https://drive.google.com/file/d/1xrxK9swFVlFI-WAYcccIgm0tt9RgawXD/view?usp=sharing)|
|  ckpt |         90.0         |     126.6    |   3.59   |[COVID-Net Large](https://drive.google.com/file/d/1djqWcxzRehtyJV9EQsppj1YdgsP2JRQy/view?usp=sharing)|


