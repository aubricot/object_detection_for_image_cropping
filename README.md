# Computer Vision with EOL v3 Images 
Testing different computer vision methods (object detection, image classification) to do customized, large-scale image processing for [Encyclopedia of Life v3 database images](https://eol.org/pages/2913056/media).   
*Last updated 31 October 2020*

<p align="center">
<a href="url"><img src="https://github.com/aubricot/computer_vision_with_eol_images/blob/master/object_detection_for_image_cropping/images/banner.jpg" align="middle" width="900" ></a></p>   

<p align="center">
<sub><sup>Images a-c are hosted by Encyclopedia of Life (a. <a href="http://upload.wikimedia.org/wikipedia/commons/a/af/Choeronycteris_mexicana%2C_Mexican_long-tongued_bat_%287371567444%29.jpg"><i>Choeronycteris mexicana</i></a>, licensed under <a href="https://creativecommons.org/licenses/by/2.0/legalcode"></a>CC BY 2.0</a>, b. <a href="https://calphotos.berkeley.edu/cgi/img_query?seq_num=81811&one=T"><i>Hippotion celerio</i></a>, licensed under <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/">CC BY-NC-SA 3.0</a>, c. <a href="https://content.eol.org/data/media/7e/b3/54/542.16276541578.jpg"><i>Cuculus solitarius</i></a> (left) and <a <i>Cossypha caffra</i></a> (right)</a>, licensed under <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC BY-SA 2.0</a>).</sup></sub>

The Encyclopedia of Life (EOL) is an online biodiversity resource that seeks to provide information about all ~1.9 million species known to science. A goal for the latest version of EOL (v3) is to better leverage the older, less structured image content. To improve discoverability and display of EOL images, automated image processing pipelines that use computer vision with the goal of being scalable to millions of diverse images are being developed and tested.   

## Project Structure
<p align="center">
<a href="url"><img src="https://github.com/aubricot/computer_vision_with_eol_images/blob/master/object_detection_for_image_cropping/images/3093378.jpg" align="right" width="400" ></a></p> 

* **Object detection for image cropping**: Three object detection frameworks (Faster-RCNN Resnet 50 and either SSD/R-FCN/Faster-RCNN Inception v2 detection via the Tensorflow Object Detection API and YOLO via Darkflow) were used to perform customized, large-scale image processing for different groups of animals (birds, bats, butterflies & moths, beetles, frogs, carnivores, snakes & lizards) found in the Encyclopedia of Life v3 database. The three frameworks differ in their speeds and accuracy: YOLO has been found to be the fastest but least accurate, while Faster RCNN was found to be the slowest but most accurate, with MobileNet SSD and R-FCN falling somewhere in between (Lin et al. 2017, Hui 2018, Redmon and Farhadi 2018). The model with the best trade-off between speed and accuracy for each group was selected to generate final cropping data for EOL images. The location and dimensions of the detected animals within each framework are used to crop images to square dimensions that are centered and padded around the detection box. For <ins>birds</ins>, pre-trained object detection models were used. For <ins>bats and butterflies & moths</ins>, object detection models were custom-trained to detect one class (either bats or butterflies & moths) using EOL user-generated cropping data (square coordinates around animal(s) of interest within each photo). For <ins>beetles, frogs, carnivores and snakes & lizards</ins>, object detection models were custom-trained to detect all classes simultaneously using EOL user-generated cropping data.  

<p align="right"> 
<sub><sup> Screenshot of object detection results using trained multitaxa detector model displayed in a Jupyter Notebook running in Google Colab. Image is hosted by Encyclopedia of Life (<a href="https://content.eol.org/data/media/28/09/c5/18.https___www_inaturalist_org_photos_1003389.jpg"><i>Lampropeltis californiae</i></a>, licensed under <a href="https://creativecommons.org/licenses/by-nc/4.0/"></a>CC BY-NC 4.0</a>.</sup></sub></p>  

* **Classification for image tagging**: Pre-trained and custom-built classification frameworks will be used to perform automated, large-scale image processing for different classes of images (flowers, maps/labels/illustrations, image ratings) found in the Encyclopedia of Life v3 database. Image tagging will improve EOL user experience by allowing users to search for features within images that are not already noted in metadata, as well as improving gallery display and API user functionality with increased image sorting abilities. The process of image classification is slightly different than object detection (Sharma 2019). While object detection includes classification and localization of the object of interest, image classification only includes the former step. Classification will be used to identify images with flowers present, images of maps/collection labels/illustrations, and to generate image quality ratings. For each of the three classifiers, a research phase is first used to identify available datasets and pre-trained classification models to be used for developing processing pipelines. For the <ins>flower classifier</ins>, pre-trained MobileNetSSD v2 (a faster, smaller, less accurate model by Sandler et al. 2018) and Inception v3 (a slower, larger, more accurate model by Szegedy et al. 2015) classification models were used, along with a custom-built classifier. Flower models were trained to classify flower images into flower, fruit, entire, branch, stem or leaf classes using the [PlantCLEF 2016 Image dataset](https://www.imageclef.org/lifeclef/2016/plant) (Goeau et al. 2016).  For the <ins>flower/fruit classifier</ins>, pre-trained MobileNetSSD v2 (a faster, smaller, less accurate model by Sandler et al. 2018) and Inception v3 (a slower, larger, more accurate model by Szegedy et al. 2015) classification models were used. Flower models were trained to classify Angiosperm images into flower/fruit or not flower/fruit classes using manually-sorted EOL images as training data. For the <ins>image type classifier</ins>, pre-trained MobileNetSSD v2 (a faster, smaller, less accurate model by Sandler et al. 2018) and Inception v3 (a slower, larger, more accurate model by Szegedy et al. 2015) classification models were used. Image rating models were trained to classify any EOL image into map, herbarium sheet, phylogeny, illustration, or none using training data from Wikimedia commons, Flickr BHL, and EOL. [To be developed: image quality rating classifiers].

Results from object detection and image classification tasks will be used to inform future large-scale image processing and user features for EOLv3 images.

## Getting Started  
Except for .py files, all files in this repository are run in [Google Colab](https://research.google.com/colaboratory/faq.html) (Google Colaboratory, "a free cloud service, based on Jupyter Notebooks for machine-learning education and research"). Using Colab, everything runs entirely in the cloud (and can link to Google Drive) and no local software or library installs are requried. Working in the cloud also makes it easier to collaborate if multiple users are on different operating systems. 

## References
* [Encyclopedia of Life](http://eol.org) 
* [Goeau et al. 2016](http://ceur-ws.org/Vol-1609/16090428.pdf). Plant identification in an open-world (LifeCLEF 2016). CEUR Workshop Proceedings. 
* [Hui 2018](medium.com/@jonathan_hui/object-detection-speed-and-accuracy-comparison-faster-r-cnn-r-fcn-ssd-and-yolo-5425656ae359). Object detection: speed and accuracy comparison (Faster R-CNN, R-FCN, SSD, FPN, RetinaNet and YOLOv3). Medium. 27 March 2018.   
* [Lin et al. 2015](https://arxiv.org/pdf/1405.0312.pdf). Microsoft COCO: Common Objects in Context.  
* [PlantCLEF 2016 Image dataset](https://www.imageclef.org/lifeclef/2016/plant)
* [Redmon and Farhadi 2018](https://arxiv.org/pdf/1804.02767.pdf). YOLOv3: An Incremental Improvement. 
* [Sandler et al. 2018](https://arxiv.org/abs/1801.04381). Inverted Residuals and Linear Bottlenecks: Mobile Networks for Classification, Detection and Segmentation." arXiv.
* [Sharma 2019](medium.com/analytics-vidhya/image-classification-vs-object-detection-vs-image-segmentation-f36db85fe81). Image Classification vs. Object Detection vs. Image Segmentation. Medium. 23 Feb 2020.
* [Szegedy et al. 2015](https://arxiv.org/abs/1512.00567). Rethinking the Inception Architecture for Computer Vision. arXiv.
