# ExtremeEarth-Datasets
![deep learning](http://earthanalytics.eu/img/general/DL2.jpg)

ExtremeEarth advances the state of the art in the area of AI, by developing distributed scale-out deep learning techniques for the classification of remote sensing images based on architectures that can effectively exploit the spatial, spectral, temporal and multimodal properties of Sentinel data.  In order to train our models we need large amounts of accurate and precise data. Satellite remote sensing is currentlly lacking such datasets and from an operational viewpoint it is not feasible to assume the availability of enough ground truth or annotated labeled data for training a deep network. In ExtremeEarth, we will developed tools to generate EO training datasets by enlarging existing datasets currently in development by partner DLR and by leveraging existing cartographic/thematic products which are now available at continental or planetary scale.

<br>
<br>
<br>
<br>

## Food Security
![crops](http://earthanalytics.eu/img/food/sat-img-field-2.jpg)

### Model
To perform the accurate crop type mapping needed in the Food Security use case, an ad-hoc deep learning algorithm has been developed for generating both crop boundary and crop type maps. The deep learning models take as input the long time series of Sentinel-2 representing the whole crop year (i.e., the period from one year's harvest to the next for an agricultural commodity) and the large training set extracted from existing thematic products available in the considered study area.

The considered model is a peculiar type of Recurrent Neural Network (RNN), namely a Long Short Term Memory (LSTM) deep learning algorithm able to capture the temporal correlation of sequential data such as the considered time series of multispectral images. The preliminary experimental analysis carried out in Austria demonstrates that the considered deep learning architecture allows the accurate characterization of the crop types. To assess the effectiveness of the proposed architecture and verify its capability of scaling to PB of Sentinel-2 data, the model has been tested on: (i) areas spatially disjoint from the ones where training data have been extracted, (ii) tiles where no training data were available and (iii) the 2018 Land Use and Cover Area frame Statistical survey (LUCAS) database (i.e., totally independent validation).

### Training Dataset
To face the lack of labeled training databases, publicly available thematic products at the country level were used to generate a large set of weak reference data. We considered the Austrian crop type map, whose official name is INVEKOS Schläge, which is based on the farmer declarations (for the crop types) and the administrative Geographic Information System (GIS) for the field geometry.
The time series of Sentinel-2 images together with the Austrian crop type map were used to generate the first version of the weak training database made up of ~1M labels. In particular, we considered 16 Sentinel-2 tiles located in Austria, for a total amount of ~1.8TB of data representing the time series of images acquired from Sep. 2017 to Sep. 2018. However, the focus of the application development will be extended to the whole Danube River catchment which is covered by 118 Sentinel-2 tiles, ~12TB of data. The prior probabilities of the crop types present in the scene are derived from the considered thematic product. This condition allows us to generate a statistically balanced training set by considering a stratified random sample approach. This sampling strategy extracts a number of samples per crop type proportional to the number of crops associated with that type in the considered study area.

### Download
[TimeSen2Crop: a Million Labeled Samples Dataset of Sentinel 2 Image Time Series for Crop Type Classification (UNITN)](https://zenodo.org/record/4715631#.YLTR0JMzZdB).

<br>
<br>
<br>
<br>

## Polar Regions
![ice](http://earthanalytics.eu/img/polar/meltIce.jpg)

### Models
In order to provide accurate sea ice mapping needed in the Polar use case, we have developed a deep learning framework based on convolutional neural networks (CNNs). We focused our attention on this approach because of its versatility when dealing with records acquired by multiple remote sensing devices and sources of information to enhance the physical interpretation of the data. Indeed, recalling that the primary source of information for sea ice analysis is provided by the SAR records acquired by Sentinel-1, this enabled our analysis to cope with incidence angle effects (i.e., the pixels with smaller incidence angles have larger signal reflectance) and the sensitivity of SAR signals to sea surface conditions such as wind speed, moisture, surface roughness, snow cover and salinity.
To model the effects of incidence angle, we created a patch-based training dataset and included incidence angle within the input record as an image channel. We modeled an ad-hoc CNN architecture and tested it on our collected SAR dataset. Moreover, we consider a VGG-16 model for sea-ice classification, considering both transfer learning and re-training from the scratch. We also tested a modified architecture of the VGG-16 model by removing the max-pooling layers to increase the spatial resolution of the analysis.

To take into account the scarcity of data (either in terms of quality and quantity) we can use for training, we used an augmentation technique to reduce the effect of overfitting during the training process. The preliminary results obtained when investigating the area north of Svalbard islands and around Danmarkshavn in Greenland showed the accuracy and reliability of the proposed approach. Moreover, these tests are particularly meaningful to assess the ability of the proposed approach to scaling up to the PB size of the Sentinel-1 archive, and hence ensure the operational use of the methods.
Training Datasets

We focused on high-resolution mapping from SAR, using Sentinel-1 data as our primary source of data. However, auxiliary data from other high-resolution SAR satellites (TerraSAR-X, Radarsat 2), and/or co-located optical data (Sentinel-2&3, Landsat) may support the creation of the training database. The task of creating a training database for sea ice classification has been approached from two angles: (1) Expert analysis of Sentinel-1 with near-simultaneous optical data from Sentinel-2&3, covering the European Arctic (UiT) and (2) Active learning using data from Belgica Bank based solely on Sentinel-1 data (DLR).
The two cases have slightly different objectives. In the case of near-simultaneous SAR and optical data, the aim is to have independent measurements from a different sensor as reference when labelling the SAR images. The labelling still relies on the analyst’s experience, but optical data is often easier to interpret than the SAR data, which increases confidence in the labels. In the case of active learning, the idea is to exploit the experience of the ice analyst in interpreting measurements from a single sensor (Sentinel-1 in this case). This allows us to make use of the vast amounts of data, although without independent ground truth.

The produced dataset for the first case consisted of 31 Sentinel-1 images with HH and HV polarization intensities, as well as incidence angle records. Each image has size 800MB, summing up to 2.4GB. Polygons of pixels have been manually labeled in six classes (i.e., open water, newly formed ice, brash/pancake ice, young ice, thin to medium first year ice, thick or deformed ice -, according to the WMO nomenclature), using co-registered optical images (Sentinel-2&3) with small time-gaps to the Sentinel-1 acquisitions, so to achieve high reliability on the labeled samples. Within these polygons, patches of pixels have been generated with stride 10 to create a suitable dataset for deep learning analysis. Three different levels of patch size have been considered: i) 10x10 pixels (for a total of 18,441 patches); ii) 20x20 pixels (for a total of 16,574 patches); iii) 32x32 pixels (for a total of 14,747 patches).
For the second case, DLR has processed and analyzed 24 Sentinel-1 images (HH + HV polarization) each image with approx. 800MB, summing up to 1.9GB. Based on their methods (classical machine learning based on active learning that learns from the user expert knowledge or hybrid methods based on Latent Dirichlet Allocation and CNN) we generated a benchmarking dataset at two different levels: (1) at intermediate level (considering patches at the size of 4x4 pixels with 11 topics / vocabulary) with a total number of patches at this level of 62,400,000 (2,600,000 patches / image), and (2) at final level (considering patches at the size of 256x256 pixels with 10 semantic labels) with a total number of patches equal to 156,000 (6500 patches / image).
A third dataset was also made available that consists of sea ice and iceberg analysis data prepared for the ExtremeEarth project (METNO). The intention is that this dataset is useful for training and validating automated methods for processing satellite images, although it could also be applied to ice analyst training. The region chosen for the analysis was Danmarkshavn on the east coast of Greenland. This has a wide variety of sea ice and iceberg conditions, and was continuously monitored by key European Sentinel satellites during 2018. The dataset consists of 12 days (approximately monthly) throughout the year, covering winter and spring, summer melt, and autumn freeze-up. The presence of extensive (land) fast ice in the area ensures that classifications on those areas can be applied to additional dates. The sea ice classification includes primary and secondary ice types, including partial concentrations and form. The initial version contains basic ice/water classification, and otherwise the polygons remain unclassified for sea ice concentrations, ice type and form. This information will be added in later versions.

### Download
1. [Synthetic-Aperture Radar (SAR) based Ice types/Ice edge dataset for deep learning analysis (UiT)](https://dataverse.no/dataset.xhtml?persistentId=doi:10.18710/QAYI4O).
2. [Semantic Sea-Ice Classification for Belgica Bank in Greenland (DLR)](https://zenodo.org/record/5075448#.YZtWZr1BwsN).
3. [Sea-Ice Data Content Representation Based on Latent Dirichlet Allocation for Belgica Bank in Greenland (DLR)](https://zenodo.org/record/5075861#.YZtWkr1BwsN).
4. [Sea ice and iceberg analysis training dataset (METNO) - version 1](https://zenodo.org/record/3695276#.YZtWtb1BwsP).
5. [Sea ice and iceberg analysis training dataset (METNO) - version 2](https://zenodo.org/record/4683174#.YZtWtb1BwsN).
