# Object Detection for Image Cropping 
Testing different existing object detection frameworks (using Faster-RCNN and SSD detection via the Tensorflow Object Detection API and YOLO via Darkflow) as a method to do customized, large-scale image processing for different groups of animals (birds, bats, butterflies & moths). 
*Last updated 12 February 2020*

Testing different existing object detection frameworks (using Faster-RCNN and SSD detection via the Tensorflow Object Detection API and YOLO via Darkflow) as a method to do customized, large-scale image processing for different groups of animals (birds, bats, butterflies & moths) in the [Encyclopedia of Life v3 database](eol.org). The three frameworks differ in their speeds and accuracy: YOLO has been found to be the fastest but least accurate, while Faster RCNN was found to be the slowest but most accurate, with MobileNet SSD falling somewhere in between (Lin et al. 2017, Hui 2018, Redmon and Farhadi 2018). Using the location and dimensions of the detected animals within each framework, images will be cropped to square dimensions that are centered and padded around the detection box. 

**Project Structure**
* **Aves**: First, the frameworks are tested with pre-trained models for "out of the box" inference on images of birds (Aves) of varying dimensions and resolutions. The model with the best trade-off between speed and accuracy - YOLO - was selected and used to generate cropping data for sets of 1,000 and 20,000 EOL bird images. Detection boxes were then consolidated to only one box per image, converted to square, and padded by 11% to ensure that beaks are not cropped out of images.

* **Chiroptera**: Next, they are trained for custom object detection of bats (Chiroptera) from images. For training data, EOL user-generated cropping data is used instead of traditional image annotation techniques. Train images are augmented using the [imgaug library](https://github.com/aleju/imgaug). Image augmentation is used to increase training data sample size and diversity to reduce overfitting when training object detection models. Model accuracy is compared  for test images using mAP (mean average precision, a standard performance measure used to evaluate object detection models) and AR (average recall). SSD and Faster-RCNN models automatically calculate mAP and AR, and these are calculated for YOLO using [mAP](https://github.com/Cartucho/mAP#create-the-ground-truth-files). The model with the best trade-off between speed and accuracy - Faster-RCNN - was selected and used to generate cropping data for sets of 1,000 and 20,000 EOL bat images. Detection boxes were then consolidated to only one box per image and converted to square.

* **Lepidoptera**: Last, they are trained for custom object detection of butterflies and moths (Lepidoptera). For training data, EOL user-generated cropping data is used instead of traditional image annotation techniques.  Train images are augmented using the [imgaug library](https://github.com/aleju/imgaug). Image augmentation is used to increase training data sample size and diversity to reduce overfitting when training object detection models. Model accuracy is compared for test images using mAP (mean average precision, a standard performance measure used to evaluate object detection models) and AR (average recall). SSD and Faster-RCNN models automatically calculate mAP and AR, and these are calculated for YOLO using [mAP](https://github.com/Cartucho/mAP#create-the-ground-truth-files). The model with the best trade-off between speed and accuracy - [to be determined] - was selected and used to generate cropping data for a set of 20,000 EOL moth and butterfly images. Detection boxes were then consolidated to only one box per image and converted to square.   

Results from the three different object detection models used for the different animal groups will be used to inform future object detection efforts and large-scale image processing for EOLv3 images.

## Getting Started  
Except convert_bboxdims.py files (aves_convert_bboxdims.py, chiroptera_convert_bboxdims.py, lepidoptera_convert_bboxdims.py) and merge_tsvs.py, all files in this repository are run in Google Colab (Google Colaboratory, "a free cloud service, based on Jupyter Notebooks for machine-learning education and research"). Using Colab, you run everything entirely in the cloud (and can link to Google Drive) and no local software or library installs are requried. If running locally and using a GPU, there are several softwares that need to be installed first and take up ~10 GB and a few workarounds are required if running on a Windows OS. Working in the cloud eliminates these problems and makes it easier to collaborate if multiple users are on different operating systems. If you prefer to use your local machine for object detection, refer to the [Tensorflow Object Detection API Tutorial] (https://github.com/tensorflow/models/blob/master/research/object_detection/object_detection_tutorial.ipynb). 

## Aves
**Step 1) Object Detection:** For YOLO in Darkflow, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/aves_yolo.ipynb). For SSD or Faster-RCNN with Tensorflow Object Detection API, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/aves_tf_ssd_rcnn.ipynb). SSD and Faster-RCNN models are implemented in the same notebook and only require changing a few lines of code to switch between.

**Step 2) Convert bounding boxes to square, centered image cropping coordinates and pad by 11%:** To convert bounding box coordinates to EOL crop coodinate formatting standards, use aves_convert_bboxdims.py on sample_crops.tsv exported from Google Colab in Step 1. You can export the 20,000 image crop dataset in 4 batches, then combine these using combine_tsvs.py before converting coordinates in aves_convert_bboxdims.py.

**Step 3) Display converted cropping coordinates on images:** To display converted crop coordinates from Step 2 on images and verify that the transformations in aves_convert_bboxdims.py are appropriate (or to fine tune accordingly, ex: adjust padding amount), click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/crop_coords_display_test.ipynb).

---

## Chiroptera
**Step 1) Split EOL user crops into train and test datasets:** Instead of creating image annotations from scratch, EOL user-generated cropping coordinates are used to create a training dataset (used to train the object detection models) and a test dataset (used to test performance and evaluate model accuracy). Following the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle), 80% of the original EOL crops dataset (consisting of images and cropping dimensions) is randomly selected for the train dataset and the remaining 20% is used for the test dataset. To split EOL user-generated cropping coordinates into train and test datasets, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/chiroptera_split_train_test.ipynb).

**Step 2) Pre-process train and test datasets:** Test and train datasets (consisting of images and cropping dimensions) exported from chiroptera_split_train_test.ipynb are pre-processed and transformed to formatting standards for use with YOLO via Darkflow and SSD and Faster-RCNN object detection models implemented in Tensorflow. All train and test images are also downloaded to Google Drive for future use training and testing. Before reformatting to object detection model standards, training data is augmented using the [imgaug library](https://github.com/aleju/imgaug). Image augmentation is used to increase training data sample size and diversity to reduce overfitting when training object detection models. Both images and cropping coordinates are augmented. The final train dataset consists of augmented and unaugmented images and cropping coordinates. The final test dataset contains only un-augmented images and cropping coordinates.

**Step 3) Object Detection:** For YOLO in Darkflow, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/chiroptera_train_yolo.ipynb). For SSD or Faster-RCNN with Tensorflow Object Detection API, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/chiroptera_train_tf_ssd_rcnn.ipynb). SSD and Faster-RCNN models are implemented in the same notebook and only require changing a few lines of code to switch between.

**Step 3a) Train object detection models:** Object detection models are trained until loss<1, for up to 30 hours. YOLO was trained for 3,000 epochs (250,000 steps), SSD for 450,000 steps and Faster-RCNN for 200,000 steps.

**Step 3b) Test object detection models:** Object detection models are tested on test images and model performance is measured using mAP (mean average precision) and AR (average recall). SSD and Faster-RCNN models automatically calculate these values. To calculate mAP and AR for YOLO using [mAP](https://github.com/Cartucho/mAP#create-the-ground-truth-files), click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/calculate_error_mAP.ipynb).

**Step 3c) Use chosen object detection model to generate bounding boxes around bats in EOL images:** The model with the best trade-off between speed and accuracy - Faster-RCNN - was selected and used to generate cropping data for sets of 1,000 and 20,000 EOL bat images in the Faster-RCNN training notebook here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/chiroptera_train_tf_ssd_rcnn.ipynb).

**Step 4) Convert bounding boxes to square, centered image cropping coordinates:** To convert bounding box coordinates to EOL crop coodinate formatting standards, use chiroptera_convert_bboxdims.py on chiroptera_det_crops_1000.tsv (for the 1,000 images dataset) and chiroptera_det_crops_20000.tsv (for the 20,000 images dataset) exported in Step 3c. Optional: the 20,000 image crop dataset can be exported in 4 batches (20000a.tsv, 20000b.tsv, etc.), then combined using combine_tsvs.py before converting coordinates in chiroptera_convert_bboxdims.py.

**Step 5) Display converted cropping coordinates on images:** To display converted crop coordinates from Step 4 on images and verify that the transformations in chiroptera_convert_bboxdims.py are appropriate (or to fine tune accordingly, ex: add padding), click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/crop_coords_display_test.ipynb).

---

## Lepidoptera
**Step 1) Split EOL user crops into train and test datasets:** Instead of creating image annotations from scratch, EOL user-generated cropping coordinates are used to create a training dataset (used to train the object detection models) and a test dataset (used to test performance and evaluate model accuracy). Following the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle), 80% of the original EOL crops dataset (consisting of images and cropping dimensions) is randomly selected for the train dataset and the remaining 20% is used for the test dataset. To split EOL user-generated cropping coordinates into train and test datasets, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/lepidoptera_split_train_test.ipynb).

**Step 2) Pre-process train and test datasets:** Test and train datasets (consisting of images and cropping dimensions) exported from lepidoptera_split_train_test.ipynb are pre-processed and transformed to formatting standards for use with YOLO via Darkflow and SSD and Faster-RCNN object detection models implemented in Tensorflow. All train and test images are also downloaded to Google Drive for future use training and testing. Before reformatting to object detection model standards, training data is augmented using the [imgaug library](https://github.com/aleju/imgaug). Image augmentation is used to increase training data sample size and diversity to reduce overfitting when training object detection models. Both images and cropping coordinates are augmented. The final train dataset consists of augmented and unaugmented images and cropping coordinates. The final test dataset contains only un-augmented images and cropping coordinates.

**Step 3) Object Detection:** For YOLO in Darkflow, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/lepidoptera_train_yolo.ipynb). For SSD or Faster-RCNN with Tensorflow Object Detection API, click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/lepidoptera_train_tf_ssd_rcnn.ipynb). SSD and Faster-RCNN models are implemented in the same notebook and only require changing a few lines of code to switch between.

**Step 3a) Train object detection models:** Object detection models are trained until loss<1, for up to 30 hours. YOLO was trained for [to be determined] epochs ([to be determined] steps), SSD for [to be determined] steps and Faster-RCNN for [to be determined] steps.

**Step 3b) Test object detection models:** Object detection models are tested on test images and model performance is measured using mAP (mean average precision) and AR (average recall). SSD and Faster-RCNN models automatically calculate these values. To calculate mAP and AR for YOLO using [mAP](https://github.com/Cartucho/mAP#create-the-ground-truth-files), click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/calculate_error_mAP.ipynb).

**Step 3c) Use chosen object detection model to generate bounding boxes around bats in EOL images:** The model with the best trade-off between speed and accuracy - [to be determined] - was selected and used to generate cropping data for sets of 1,000 and 20,000 EOL butterfly and moth images in the [to be determined] training notebook here [to be determined].

**Step 4) Convert bounding boxes to square, centered image cropping coordinates:** To convert bounding box coordinates to EOL crop coodinate formatting standards, use lepidoptera_convert_bboxdims.py on lepidoptera_det_crops_1000.tsv (for the 1,000 images dataset) and lepidoptera_det_crops_20000.tsv (for the 20,000 images dataset) exported in Step 3c. Optional: the 20,000 image crop dataset can be exported in 4 batches (20000a.tsv, 20000b.tsv, etc.), then combined using combine_tsvs.py before converting coordinates in lepidoptera_convert_bboxdims.py.

**Step 5) Display converted cropping coordinates on images:** To display converted crop coordinates from Step 4 on images and verify that the transformations in chiroptera_convert_bboxdims.py are appropriate (or to fine tune accordingly, ex: add padding), click here [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aubricot/object_detection_for_image_cropping/blob/master/crop_coords_display_test.ipynb).

---
---   

#### References
Cartucho 2019. mAP (mean average precision). GitHub. https://github.com/Cartucho/mAP.
Hui 2018. Object detection: speed and accuracy comparison (Faster R-CNN, R-FCN, SSD, 
FPN, RetinaNet and YOLOv3). Medium. 27 March 2018. 
medium.com/@jonathan_hui/object-detection-speed-and-accuracy-comparison-faster-r-cnn-r-fcn-ssd-and-yolo-5425656ae359.   
Jung 2019. GitHub. imgaug-doc. https://github.com/aleju/imgaug-doc. 
Lin et al. 2015. Microsoft COCO: Common Objects in Context. arXiv:1405.0312.  
Liu et al. 2015. SSD: Single Shot MultiBox Detector. arXiv:1512.02325.  
Redmon and Farhadi 2018. YOLOv3: An Incremental Improvement. arXiv:1804.02767.   
Ren et al. 2016. Faster R-CNN: Towards Real-Time Object Detection with Region Proposal 
Networks. arXiv:1506.01497.
