# DetectChange --- Satellite Image Classification & Change Detection

**Academic project · UV6.1 · 2026--2027**\
**Student:** Hiba ESSID\
**Supervisors:** JC Cexus & AB Toumi

> DetectChange explores deep-learning methods for land-use/land-cover
> classification and bi-temporal change detection using Sentinel-2
> satellite imagery.

## Overview

Satellite imagery makes it possible to characterize land use and monitor
how a region evolves over time. This project follows a two-stage
approach:

1.  **Single-date image classification:** train and evaluate a model on
    the EuroSAT dataset.
2.  **Bi-temporal change detection:** reuse the learned image
    representation to compare aligned satellite images acquired at two
    different dates and identify changed areas.

EuroSAT contains 27,000 geo-referenced image patches across 10 land-use
and land-cover classes. It is available in RGB and 13-band multispectral
configurations. It is suitable for single-date classification, but does
not itself provide annotated before/after pairs for change detection. A
separate multi-temporal dataset will therefore be selected for the
second stage.

## Project objectives

-   Prepare and process geospatial and satellite-image datasets.
-   Build a multi-class land-use/land-cover classifier.
-   Apply transfer learning with a pre-trained MobileNetV2 model.
-   Reuse the trained encoder for bi-temporal image analysis.
-   Explore a Siamese architecture with shared weights for binary change
    detection.
-   Evaluate models with metrics suited to class imbalance.
-   Document experiments so that results can be reproduced.
-   Analyze model errors, complexity and inference performance.

## Methodology

### Part A --- Single-date classification

**Dataset:** [EuroSAT](https://github.com/phelber/EuroSAT)\
**Main architecture:** MobileNetV2 pre-trained on ImageNet\
**Initial configuration:** RGB images

Planned experiments:

-   Train a classification head while keeping the pre-trained backbone
    frozen.
-   Fine-tune selected MobileNetV2 layers.
-   Optionally compare against a compact CNN baseline.

The dataset will be split into training, validation and test sets
without data leakage. Data augmentation and preprocessing choices will
be documented.

**Evaluation metrics**

-   Accuracy
-   Precision and recall
-   Macro F1-score
-   Per-class results
-   Confusion matrix
-   Number of parameters and model size
-   Inference time

The analysis will pay particular attention to class-wise performance and
misclassified examples.

### Part B --- Bi-temporal change detection

**Candidate datasets:** [OSCD](https://rcdaudt.github.io/oscd/) and
[S2MTCP](https://zenodo.org/records/4280482)\
**Main idea:** compare two aligned images of the same area acquired at
dates *t1* and *t2*.

Three approaches are planned:

1.  **Classification comparison:** compare the class predictions
    obtained independently for the two dates.
2.  **Feature comparison:** compare MobileNetV2 feature representations
    before the classification layer.
3.  **Siamese change detection:** use two branches with shared weights
    to process the two dates and predict a binary change/no-change map.

Before model training, the bi-temporal data must be co-registered,
brought to a common spatial resolution and normalized consistently.

The choice between OSCD and S2MTCP will be based on the availability of
ground-truth annotations, dataset size, learning setup and
data-preparation effort.

**Evaluation metrics**

-   F1-score for the change class
-   Intersection over Union (IoU / Jaccard)
-   Precision and recall
-   Confusion matrix

Because unchanged pixels may outnumber changed pixels, overall accuracy
alone may not adequately describe model performance.

## Optional investigations

If time permits, the project may also investigate:

-   RGB versus 13-band multispectral input.
-   The use of the near-infrared band or an index such as NDVI.
-   Robustness to seasonal variations.
-   A comparison between the MobileNetV2 Siamese model and a dedicated
    architecture such as
    [ChangeStar](https://github.com/Z-Zheng/ChangeStar).

## Tools and technologies

The project is centered on deep learning and satellite-image analysis.
The project brief identifies MobileNetV2, Sentinel-2 and EuroSAT as core
elements. The final software stack and implementation details will be
specified as the experiments progress.

## Repository structure

The structure below is a suggested organization for the implementation;
it can be adapted as the project develops.

``` text
DetectChange/
├── data/
│   ├── eurosat/
│   └── change_detection/
├── notebooks/
│   ├── 01_eurosat_exploration.ipynb
│   ├── 02_mobilenetv2_classification.ipynb
│   └── 03_bitemporal_change_detection.ipynb
├── src/
│   ├── data/
│   ├── models/
│   ├── training/
│   └── evaluation/
├── experiments/
├── results/
├── requirements.txt
└── README.md
```

> Dataset files are not included in this repository by default. Follow
> the official dataset instructions and check their terms of use before
> downloading or redistributing data.

## References and resources

### Datasets

-   [EuroSAT --- official
    repository](https://github.com/phelber/EuroSAT)
-   [EuroSAT --- TensorFlow
    Datasets](https://www.tensorflow.org/datasets/catalog/eurosat)
-   [OSCD --- Onera Satellite Change
    Detection](https://rcdaudt.github.io/oscd/)
-   [S2MTCP](https://zenodo.org/records/4280482)

### Related implementations

-   [EuroSAT: CNN, MobileNetV2 and ResNet50
    comparison](https://github.com/GreenDEins/eurosat-cnn-classification)
-   [MobileNetV2 + EuroSAT ---
    TensorFlow](https://github.com/pedro-rios66/satellite-imagery-deep-learning)
-   [MobileNetV2 + EuroSAT ---
    PyTorch](https://github.com/AbhinavKumar0000/Satellite-Image-Classification-using-Mobilenetv2)
-   [Fully Convolutional Change
    Detection](https://github.com/rcdaudt/fully_convolutional_change_detection)
-   [ChangeStar](https://github.com/Z-Zheng/ChangeStar)
-   [Awesome Remote Sensing Change
    Detection](https://github.com/yjt2018/awesome-remote-sensing-change-detection)

### Research papers

-   Helber, P., Bischke, B., Dengel, A., & Borth, D. (2019). *EuroSAT: A
    Novel Dataset and Deep Learning Benchmark for Land Use and Land
    Cover Classification*. IEEE JSTARS.
-   Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L.-C.
    (2018). *MobileNetV2: Inverted Residuals and Linear Bottlenecks*.
    CVPR, 4510--4520.
-   Daudt, R. C., Le Saux, B., Boulch, A., & Gousseau, Y. (2018). *Urban
    Change Detection for Multispectral Earth Observation Using
    Convolutional Neural Networks*. IGARSS, 2115--2118.
-   Leenstra, M., Marcos, D., Bovolo, F., & Tuia, D. (2021).
    *Self-supervised pre-training enhances change detection in
    Sentinel-2 imagery*. PRRS / ICPR, LNCS 12667.
-   Zheng, Z., Ma, A., Zhang, L., & Zhong, Y. (2021). *Change is
    Everywhere: Single-Temporal Supervised Object Change Detection in
    Remote Sensing Imagery*. ICCV, 15193--15202.
-   Daudt, R. C., Le Saux, B., & Boulch, A. (2018). *Fully convolutional
    siamese networks for change detection*. ICIP, 4063--4067.

## Project status

`<sub>`{=html}Project proposal for the 2026--2027 academic year. The
experiments and implementation are planned work; results will be added
as they become available.`</sub>`{=html}
