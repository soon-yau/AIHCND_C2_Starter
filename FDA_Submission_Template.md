# FDA  Submission

**Your Name: Dr. David Wilson**

**Name of your Device:** Pneudet 100

## Algorithm Description 

### 1. General Information

**Intended Use Statement:** 

This algorithm is intended for use on men and women from age of 1 to 95 who have know lung diseases or have existing diseases one or combination of atelectasis, cardiomegaly, consolidation, edema, effusion, emphysema, fibrosis, hernia, infiltration, mass,  nodule, pleural_thickening, pneumonia and pneumothorax.

**Indications for Use:**

Screening for pneumonia to assist radiologists in non-emergency scenario.

**Device Limitations:**

Minimum computer specification for the software is 2-core CPU, 16 GB RAM and  32GB disk.

**Clinical Impact of Performance:**

The algorithm was designed to have higher recall which means there may be more false positive. This means it is more likely for the algorithm to classify a non-pneumonia xray image as containing pneumonia. The algorithm can also produce false negative result where xray image with pneumonia may be classified as not having pneumonia.  

### 2. Algorithm Design and Function

The algorithm uses deep neural network called CNN (Convolutional Neural Network) to classify the present of pneumonia from X-ray scans. The algorithm flow is:

- Image pre-processing
- Feed image into CNN to get output
- If the CNN output exceeds confidence threshold, the image is classified as positive with pneumonia.

**DICOM Checking Steps:**

DICOM is checked to contain ensure only Xray images of chest are used. 

**Preprocessing Steps:**

Grayscale image is resized to dimension of 224 rows, 224 columns, and converted to 3 color channels. The image pixel values are normalized from [0, 255] to between range of [0,1]

**CNN Architecture:**

The base network is Resnet50 pre-trained on Imagenet data, followed by:

- Batchnormalization
- Convolutional layer, 1x1 kernel, 1024 filters, stride=1, relu activation
- Dropout
- Batchnormalization
- Convolutional layer, 1x1 kernel, 256 filters, stride=1, relu activation
- Dropout
- Average pooling of 7x7
- Batchnormalization
- Convolutional layer, 1x1 kernel, 1 filters, stride=1, sigmoid activation
- Reshape to [batch_size, 1]


### 3. Algorithm Training

**Parameters:**
* Types of augmentation used during training, applied only to training dataset

  * random height shift of up to +-15% of images height
  * random width shift of up to +-15% of images height
  * random rotation of +-25 degrees
  * random sheer transformation of up to +-10%
  * random zoom of up to +-10%

* Batch size = 64

* Optimizer learning rate = 0.0001 with decrease of 10% in every epoch after second epoch

* Layers of pre-existing architecture that were frozen. 

  None

* Layers of pre-existing architecture that were fine-tuned

  All layers.

* Layers added to pre-existing architecture

  As described in above.

<< Insert algorithm training performance visualization >> 

![train_curve](/home/soon/github/AIHCND_C2_Starter/images/train_curve.png)

<< Insert P-R curve >>

![auc](/home/soon/github/AIHCND_C2_Starter/images/auc.png)

![pr](/home/soon/github/AIHCND_C2_Starter/images/pr.png)

**Final Threshold and Explanation:**

The threshold of 0.1 is selected to have higher recall.

### 4. Databases
 (For the below, include visualizations as they are useful and relevant)

The dataset is obtained from [kaggle website](https://www.kaggle.com/nih-chest-xrays/data) that contains 112,120 frontal-view chest X-ray PNG images in 1024*1024 resolution and include 14 common thoracic pathologies: 
- Atelectasis 
- Consolidation
- Infiltration
- Pneumothorax
- Edema
- Emphysema
- Fibrosis
- Effusion
- Pneumonia
- Pleural thickening
- Cardiomegaly
- Nodule
- Mass
- Hernia 

Below is the distribution of diseases in the dataset. Please note that they are not mutually exclusive, meaning one patient can have multiple diseases labelled for a x-ray image.

![](/home/soon/github/AIHCND_C2_Starter/images/disease_dist.png)



Age distribution for people with and without pneumonia

![](/home/soon/github/AIHCND_C2_Starter/images/age.png)

There are total of 1431 samples with pneumonia and 110689 samples without pneumonia in the dataset. Age distribution of dataset is 56.5% male and 43.5% female.

**Description of Training Dataset:** 

Validation is a 89696 samples contain 50%  positive cases of pneumonia and 50% cases with no pneumonia but may or not may not include other diseases.  This is sampled with replacement from dataset excluding validation dataset.

**Description of Validation Dataset:**

Validation is a 22424 samples which contain 1.27% of true positive.


### 5. Ground Truth

 To create these labels, the authors used Natural Language Processing to text-mine disease classifications from the associated radiological reports. The labels are expected to be >90% accurate and suitable for weakly-supervised learning.

### 6. FDA Validation Plan

**Patient Population Description for FDA Validation Dataset:**

The sample population should be taken from men and women distributed between the age of 1 and 100 . Image modality shall be Xray image showing chest in both front and back views. The sample can include people with or without prio diagnosis of pneumonia and other lung diseases.

**Ground Truth Acquisition Methodology:**

X-ray images validated by at least 3 different radiologists.

**Algorithm Performance Standard:**

Recall score. 