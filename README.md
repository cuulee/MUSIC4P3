#  MUSIC for P3 dataset

## Overview
**MU**ltiband **S**atellite **I**magery for object  **C**lassification   (**MUSIC**)  to detect **P**hotovoltaic **P**ower **P**lants 


The number of photovoltaic power plants is growing so rapidly that we must rely on satellitel observations and efficient machine learning methods for the global monitoring. **MUSIC** for P3 (**P**hotovoltaic **P**ower **P**lants) is a  training and validation dataset generated by  [AIRC/AIST](http://www.airc.aist.go.jp/en/) to support such a  global survey of photovoltaic power plants.  


We have picked up the photovoltaic power plants listed on [the website of Electrical Japan](http://agora.ex.nii.ac.jp/earthquake/201103-eastjapan/energy/electrical-japan/type/8.html.ja) only if they generated more than 5MW electricity and the construction had been completed by 2015. The multiband satellite images of these target areas, taken by [Landsat-8](https://landsat.usgs.gov/landsat-8), were cropped into a 16 × 16 grid covering a 480 × 480 meter area as shown below. 

![fig:MUSIC4P3 image patch example](https://github.com/gistairc/MUSIC4P3/blob/master/fig.jpg "megasolar image patch example")  


These patch images are classified as “positives” if the solar panels cover more than 20% of the total areas, while patches with no solar panels are classified as “negatives”. The rest with the intermediate coverage (0~20%) were neither “positives” and “negatives”. 


You can download the **MUSIC** for P3 dataset with two different format (HDF5 and GeoTiff) along with the source code for the detection and classification. More detailed exaplanations can be found in the following papers.

[1] *Tomohiro Ishii, Edgar Simo-Serra, Satoshi Iizuka, Yoshihiko Mochizuki, Akihiro Sugimoto, Ryosuke Nakamura, Hiroshi Ishikawa ,"Detection by Classification of Buildings in Multispectral Satellite Imagery," ICPR 2016.* (http://www.f.waseda.jp/hfs/IshiiICPR2016.pdf)  

[2] *Nevrez Imamoglu, Motoki Kimura, Hiroki Miyamoto, Aito Fujita, Ryosuke Nakamura,"Solar Power Plant Detection on Multi-Spectral Satellite Imagery using Weakly-Supervised CNN with Feedback Features and m-PCNN Fusion," BMVC 2017.* (https://arxiv.org/abs/1704.06410)  

It should be noted here that the initial dataset (V1) is contaminated by small-scale plants not included in the original inventory. After publishing these papers, we have checked all the false positives through in-situ survey or high-resolution imagery. In fact, some of "false positives" are found to be relatievely small photovoltaic power plants unlisted in the original database. We produced updated V2 dataset by correcting these misclassification. The performance of our previous works was estimated with  V1 dataset, but more accurate estimate can be achieved by using V2.



## Download  
**IMPORTANT** -- Please read the [Terms of Use](https://github.com/gistairc/MUSIC4P3dataset/blob/master/LICENSE.md) before downloading the MUSIC4P3 dataset.


#### hdf5
HDF5 version (for using Torch version code) of the dataset can be downloaded from [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_hdf.zip) (4.6GB) .  
Or type the following in the terminal.  

```
$ wget http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_hdf.zip
$ unzip MUSIC4P3data_hdf.zip
```

Schematic of the directory configuration in the unzipped files is as follows:  
```
./resource/
train/  
	LC81060302015147LGN00.hdf5
	LC81070302015266LGN00.hdf5 ...
val/
	LC81060302015147LGN00.hdf5
	LC81070302015266LGN00.hdf5 ...
test/
	LC81060302015147LGN00.hdf5
	LC81070302015266LGN00.hdf5 ...
```

#### tiff
Tiff version (for using Chainer version code) of the dataset can be downloaded from [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_tiff.zip) (4.2GB) .  
Or type the following in the terminal.  
```
$ wget http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_tiff.zip
$ unzip MUSIC4P3data_tiff.zip
```
Schematic of the directory configuration in the unzipped files is as follows:  
```
./resource/
train/
	positive/
		LC81060302015147LGN00_274_300_2064.tiff
		LC81060302015147LGN00_274_300_13289.tiff ...
	negative/
		LC81060302015147LGN00_100_105.tiff
		LC81060302015147LGN00_100_114.tiff ...
val/
	positive/
		LC81070352015298LGN00_206_266.tiff
		LC81070352015298LGN00_374_160.tiff ...
	negative/
		LC81060302015147LGN00_2_86.tiff
		LC81060302015147LGN00_4_88.tiff ...
test/
	positive/
		LC81060302015147LGN00_79_323.tiff
		LC81060302015147LGN00_80_323.tiff ...
	negative/
		LC81060302015147LGN00_100_100.tiff
		LC81060302015147LGN00_100_101.tiff ...
```

## Source
### Torch ver  
#### Requierment
* torch7  
For torch installition see http://torch.ch/  
* torch_toolbox  
Download from https://github.com/e-lab/torch-toolbox  
Unzip it and put on it same directory.  
```
FOR EXAMPLE
	<dir>torch/
		<dir>resource
		<dir>torch-toolbox
		train-cnn.lua
		train-cnn.sh
		test-cnn.lua
		test-cnn.sh ...
```
 
Can be changed by passing in train-cnn.lua line 9.

#### Training
For trainging, type the following this code in the terminal.  

```
$ sh train-cnn.sh
```
Models are saved to "./ishiinet_p1n15/cnn_nm_epXXXX.net" (Can be changed by passing in train-cnn.sh) .  

#### Testing

Type the following command in the terminal.  

```
$ sh test-cnn.sh
```
Result file are saved to "./ishiinet_p1n15/test_th0.999_all_cm.txt".  
Can be changed by passing in this shell file.  


### Chainer ver
#### Requierment
* Python 2.7.*  
* Chainer 1.24.*  
For chainer Installition see from https://chainer.org/  

#### Training
Type the following this code in the terminal.   

```
$ sh train.sh
```
 
Models are save to ./result/ (Can be changed by passing in this shell file) . 

#### Testing
Type the following this code in the terminal.   

```
$ sh test.sh > result.log
```

Result file are created to ./result.log .  

## Acknowledgement
This dataset and source code are based on results obtained from a project commissioned by the New Energy and Industrial Technology Development Organization (NEDO).  
