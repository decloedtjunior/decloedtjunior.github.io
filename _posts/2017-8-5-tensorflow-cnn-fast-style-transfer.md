---
layout: post
sitemap: true
noindex: false
title: TensorFlow CNN for fast style transfer
categories: tensorFlow cnn fast-style-transfer
---

Style transfer allows you to take famous paintings, and recreate your own images in their styles! This implementation is based off of a combination of Gatys' A Neural Algorithm of Artistic Style, Johnson's Perceptual Losses for Real-Time Style Transfer and Super-Resolution, and Ulyanov's Instance Normalization.

[Check the github repo](https://github.com/lengstrom/fast-style-transfer)


## Create the environment
```
conda create -n style-transfer python=3.5
activate style-transfer
```

## Install Packages
```
pip install tensorflow
conda install scipy pillow
git clone https://github.com/lengstrom/fast-style-transfer.git .
```

## Download Pre-trained model and an image
```
wget https://d17h27t6h515a5.cloudfront.net/topher/2017/January/587d1865_rain-princess/rain-princess.ckpt

wget https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAyhAAAAJDk3NWVmYTIwLTAxNWQtNDcwOC05NmUxLWU1NTI5YWFjMTUwZQ.jpg -O mypic.jpg
```

## Running and check the results:
```
python evaluate.py --checkpoint rain-princess.ckpt --in-path mypic.jpg --out-path output_image.jpg

```
![_config.yml]({{ site.baseurl }}/images/2017-8-5-tensorflow-cnn-fast-style-transfer-01.jpg)
