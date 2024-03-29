---
layout: article
title: IITH-CANDLE
author: 
mode: immersive
header:
  theme: dark
aside:
  toc: true
article_header:
  type: cover
  image:
    src: /images/grid12x.png
---
<div style="text-align: right">
  <font size="2.5">
    <a href="https://sites.google.com/view/gowthamabbavaram/abbavaramgowthamreddy">Abbavaram Gowtham Reddy</a>, 
    <a href="https://beningodfrey.com">Benin Godfrey L</a>, 
    <a href="https://www.iith.ac.in/~vineethnb/">Vineeth N Balasubramanian</a> 
  </font>
</div>

[![arXiv](https://img.shields.io/badge/arXiv-2112.05746-b31b1b.svg)](https://arxiv.org/abs/2112.05746)
[![AAAI](https://img.shields.io/badge/AAAI 2022-identifier-275161.svg)](https://aaai.org/Conferences/AAAI-22/)
[![Get the dataset](https://img.shields.io/badge/Get%20the%20Dataset-4285F4?logo=googledrive&logoColor=white)](https://drive.google.com/drive/folders/11w267LWI8tbWhf1SR8kd-l6fP9WbJwNL)
[![GitHub](https://img.shields.io/github/license/causal-disentanglement/IITH-CANDLE)](https://github.com/causal-disentanglement/IITH-CANDLE/blob/main/LICENSE)

<a href="https://iith.ac.in"><img align="right" height=100px style="margin-left: 10px" src="./images/iith.png"></a>
<a href="https://lab1055.github.io"><img align="right" height=100px style="margin-left: 10px" src="./images/lab1055.png"></a>

The IITH-CANDLE dataset contains 12546 images in `.png` format. Each image is rendered by randomly placing a foreground object in a background scene. Generating images in a controlled manner allows for recording the ground truth factors of variation and hence allows us to study disentangled representation learning using both unsupervised and supervised learning.

# Poster
<object data="AAAI_POSTER.pdf" width="850" height="700" type='application/pdf'></object>

# Factors of Variation
IITH-CANDLE dataset is generated by following the data generating mechanism detailed below. All generative factors are self explanatory. $U$ denotes an unobserved confounder, viz. the subtle interactions between the *scene*'s natural light and the external *light* factor. $C$ denotes any observed confounder which is responsible for spurious correlations in data. Values taken by each of the generative factors in the causal graph are listed in the table below.

<p align="center">
  <img width="460" src="./images/datagenerator.png">
</p>

| Generative Factor | Possible Values |
| --- | --- |
| Object | Cube, Sphere, Cylinder, Cone, Torus |
| Color | Red, Blue, Yellow, Purple, Orange |
| Size | Small, Medium, Large |
| Rotation | 0◦, 15◦, 30◦, 45◦, 60◦, 90◦ |
| Light | Left, Middle, Right |
| Scene | Indoor, Playground, Outdoor, Bridge, City Square, Hall, Grassland, Garage, Street, Beach, Station, Tunnel, Moonlit Grass, Dusk City, Skywalk, Garden |

# Ground-truth information
Ground truth information for each image in the dataset is provided in `json` format. Below is a sample image and its corresponding meta data.

<img align="right" height=275px style="margin-left: 10px" src="./images/3541.png">

```json
{
    "scene": "playground",
    "lights": "middle",
    "objects": {
        "Torus_0": {
            "object_type": "torus",
            "color": "blue",
            "size": 2.5,
            "rotation": 15,
            "bounds": [
                [150,36],
                [245,66]
            ]
        }
    }
}
```

# Download IITH-CANDLE
The dataset can be downloaded from [here](https://drive.google.com/drive/folders/11w267LWI8tbWhf1SR8kd-l6fP9WbJwNL) (1.7 GB).

# Simulating and extending IITH-CANDLE
IITH-CANDLE is rendered using [Blender](https://www.blender.org) by overlaying 3D sprites on panoramic HDRi background images. Simulating a version is as simple as running `blender -b -noaudio -P candle_simulator.py`.

Don't like the torus? Want a monkey there instead? Simply create the sprites and add them to the simulator. Code and instructions are at [the candle-simulator repository](https://github.com/causal-disentanglement/candle-simulator).

# Evaluation Metrics
We also propose two evaluation metrics in our paper to study *causal disentanglement*. We look at latent variable models (e.g., $\beta$ VAE) as disentangled causal processes (Suter et al, ICML 2019) and propose two evaluation metrics called Unconfoundedness ($UC$) and Counterfactual Generativeness ($CG$) to measure causal disentanglement using the notion of causally responsible generative factors in an image. The $UC$ metric evaluates how well distinct generative factors $G_i$ (e.g., *object, color*) are captured by distinct sets of latent dimensions $Z_I$ with no overlap. If a latent variable model encodes the underlying generative factor $G_i$ of an image $x$ as a set of latent dimensions $Z_I^x$, we define $UC$ measure as: $UC := 1 - E_{x} (\frac{1}{S} \sum_{I,J} \frac{|Z_I^x \cap Z_J^x|}{|Z_I^x \cup Z_J^x|})$. where $S= \binom{n}{2}$ is the number of pairs of generative factors $(G_i,G_j), i\ne j$. When a latent variable model achieves good $UC$ score, we can perform interventions on any specific $Z_I$ to generate counterfactual instances without any confounding effect. We call this property as counterfactual generativeness. We define $CG$ metric as the difference in causal effects of $Z_I$ and $Z_{\setminus I}$ on the generated counterfactual images by latent variable modles w.r.t. the generative factor $G_i$. Please refer to the paper for more details.

# Experiments
The code and instructions to reproduce experiments in the paper can be found at (a fork of) [disentanglement_lib](https://github.com/causal-disentanglement/disentanglement_lib).

# Comparison With Existing Datasets
In the table below we compare IITH-CANDLE dataset with some popular datasets in disentanglement representation learning and OOD generalization tasks.

|Dataset|Depth of Underlying Causal Graph| 3D | Realistic | Presence of Foreground Object| Foreground Object Not Centered | Complex Background | Confounders|
| ------------- | ------------- |---|---|---|---|---|---|
|dSprites|1| :x:|:x:|:heavy_check_mark:|:heavy_check_mark:|:x:| :x:|
|Noisy dsprites|1|:x:|:x:|:heavy_check_mark:|:heavy_check_mark:|:x:|:x:|
|Scream dsprites|1|:x:|:x:|:heavy_check_mark:|:heavy_check_mark:|:x:|:x:|
|SmallNORB|1    |:heavy_check_mark:|:x:|:heavy_check_mark:|:x:|:x:|:x:|
|Cars3D|1       |:heavy_check_mark:|:x:|:heavy_check_mark:|:x:|:x:|:x:|
|3Dshapes|1     |:heavy_check_mark:|:x:|:heavy_check_mark:|:x:|:x:|:x:|
|Falcor3D|1     |:heavy_check_mark:|:x:|:x:|:x:|:heavy_check_mark:|:x:|
|Isaac3D|1      |:heavy_check_mark:|:x:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:x:|
|MPI3D-toy|1    |:heavy_check_mark:|:x:|:heavy_check_mark:|:heavy_check_mark:|:x:|:x:|
|MPI3D-realistic|1|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:x:|:x:|
|MPI3D-real|1   |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:x:|:x:|
|Imagenet-C|N/A|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|N/A|
|CIFAR-10/100-C|N/A|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|N/A|
|Colored-MNIST|N/A|:heavy_check_mark:|:x:|:heavy_check_mark:|:x:|:x:|N/A|
|PACS|N/A|N/A|N/A|:heavy_check_mark:|:heavy_check_mark:|N/A|N/A|
|Office-Home|N/A|N/A|N/A|:heavy_check_mark:|:heavy_check_mark:|N/A|N/A|
|<span style="color:blue">**IITH-CANDLE**</span>|<span style="color:blue">**2**</span>|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|

# Abstract
Representation learners that disentangle factors of variation have already proven to be important in addressing various real world concerns such as fairness and interpretability. Initially consisting of unsupervised models with independence assumptions, more recently, weak supervision and correlated features have been explored, but without a causal view of the generative process. In contrast, we work under the regime of a causal generative process where generative factors are either independent or can be potentially confounded by a set of observed or unobserved confounders. We present an analysis of disentangled representations through the notion of disentangled causal process. We motivate the need for new metrics and datasets to study causal disentanglement and propose two evaluation metrics and a dataset. We show that our metrics capture the desiderata of disentangled causal process. Finally, we perform an empirical study on state of the art disentangled representation learners using our metrics and dataset to evaluate them from causal perspective.

# Paper PDF

An arXiv preprint is available [here](https://arxiv.org/abs/2112.05746).

# How to cite our work
If you use *IITH-CANDLE*, please consider citing:
```
@article{iithcandle, 
title={On Causally Disentangled Representations},  
journal={Proceedings of the AAAI Conference on Artificial Intelligence}, 
author={Abbavaram Gowtham Reddy and Benin Godfrey L and Vineeth N Balasubramanian}, 
year={2022},
month={February}
}
```

# License
This work is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
