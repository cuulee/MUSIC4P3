#  MUSIC for P3 dataset

## Overview

The number of photovoltaic power plants is growing so rapidly that we must rely on satellitel observations and efficient machine learning methods for the global monitoring. **MUSIC** for P3 (**P**hotovoltaic **P**ower **P**lants) is a  training and validation dataset generated by  [AIRC/AIST](http://www.airc.aist.go.jp/en/) to support such a  global survey of photovoltaic power plants.  


We have picked up the photovoltaic power plants listed on [the website of Electrical Japan](http://agora.ex.nii.ac.jp/earthquake/201103-eastjapan/energy/electrical-japan/type/8.html.ja) only if they generated more than 5MW electricity and the construction had been completed by 2015. The multiband satellite images of these target areas, taken by [Landsat-8](https://landsat.usgs.gov/landsat-8), were cropped into a 16 × 16 grid covering a 480 × 480 meter area as shown below. 

![fig:MUSIC4P3 image patch example](https://github.com/gistairc/MUSIC4P3/blob/master/fig.jpg "megasolar image patch example")  


These patch images are classified as “positives” if the solar panels cover more than 20% of the total areas, while patches with no solar panels are classified as “negatives”. The rest with the intermediate coverage (0~20%) were neither “positives” and “negatives”. 


You can download the **MUSIC** for P3 dataset with two different format ([HDF5](https://github.com/gistairc/MUSIC4P3#hdf5) and [GeoTiff](https://github.com/gistairc/MUSIC4P3#tiff)) along with the source code for the detection and classification. More detailed exaplanations can be found in the following papers.

[1] *Tomohiro Ishii, Edgar Simo-Serra, Satoshi Iizuka, Yoshihiko Mochizuki, Akihiro Sugimoto, Ryosuke Nakamura, Hiroshi Ishikawa ,"Detection by Classification of Buildings in Multispectral Satellite Imagery," ICPR 2016.* (http://www.f.waseda.jp/hfs/IshiiICPR2016.pdf)  

[2] *Nevrez Imamoglu, Motoki Kimura, Hiroki Miyamoto, Aito Fujita, Ryosuke Nakamura,"Solar Power Plant Detection on Multi-Spectral Satellite Imagery using Weakly-Supervised CNN with Feedback Features and m-PCNN Fusion," BMVC 2017.* (https://arxiv.org/abs/1704.06410)  

##### Updated dataset (V2)
It should be noted here that the "negatives" in the initial dataset (V1) is contaminated by small-scale photovoltaic power plants not included in the [original inventory](http://agora.ex.nii.ac.jp/earthquake/201103-eastjapan/energy/electrical-japan/type/8.html.ja). After publishing these papers, we have checked all the false positives through in-site survey or high-resolution imagery. In fact, some of "false positives" are found to be relatievely small photovoltaic power plants unlisted in the original database. Therefore, we've produced updated V2 dataset by correcting these misclassification. The performance of our previous works was estimated with  V1 dataset, but more accurate estimate can be achieved by using this updated dataset V2.



## Download  
**IMPORTANT** -- Please read the [Terms of Use](https://github.com/gistairc/MUSIC4P3/blob/master/LICENSE.md) before downloading the MUSIC4P3 dataset.


#### hdf5
HDF5 version (for using [Torch](https://github.com/gistairc/MUSIC4P3#torch-ver) version code) of the dataset can be downloaded from [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_hdf.zip) (4.6GB) .  
Or type the following in the terminal.  

```
$ wget http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_hdf.zip
$ unzip MUSIC4P3data_hdf.zip
```

The directory configuration in the unzipped folder:  
```
./torch/resource/
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


[V2](https://github.com/gistairc/MUSIC4P3/blob/master/README.md#update-data-v2) dataset is [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_hdf_v2.zip).


#### tiff
Tiff version (for using [Chainer](https://github.com/gistairc/MUSIC4P3#chainer-ver) version code) of the dataset can be downloaded from [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_tiff.zip) (4.2GB) .  
Or type the following in the terminal.  
```
$ wget http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_tiff.zip
$ unzip MUSIC4P3data_tiff.zip
```
The directory configuration in the unzipped files is as follows:  
```
./chainer/resource/
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

[V2](https://github.com/gistairc/MUSIC4P3/blob/master/README.md#update-data-v2) dataset is [here](http://data.airc.aist.go.jp/MUSIC4P3dataset/MUSIC4P3data_tiff_v2.zip).

## Source
### Torch ver  
#### Requierment
* torch7  
For torch installition see http://torch.ch/  
* torch_toolbox  
Download from https://github.com/e-lab/torch-toolbox  
Unzip and put the it on the same directory.  
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
 
#### Training
For trainging, type the followingin the terminal.  

```
$ sh train-cnn.sh
```
Models are saved to "./ishiinet_p1n15/cnn_nm_epXXXX.net" (the output file name is specified in  train-cnn.sh) .  

#### Testing

Type the following in the terminal.  

```
$ sh test-cnn.sh
```
Result file are saved to "./ishiinet_p1n15/test_th0.999_all_cm.txt".  
Can be changed by passing in this shell file.  
If you want to the same result from paper\[1\], please use this [pre training model.](http://data.airc.aist.go.jp/MUSIC4P3dataset/cnn_nm_ep1930.net)


### Chainer ver
#### Requierment
* Python 2.7.*  
* Chainer 1.24.*  
For chainer Installition, see from https://chainer.org/  

#### Training
Type the following in the terminal.   

```
$ sh train.sh
```
 
Models are save to ./result/ (the output directory is specified in train.sh) . 

#### Testing

Type the following  in the terminal. 

```
$ sh test.sh > result.log
```

Result file are created to ./result.log .  
If you want to the same result from paper\[1\], please use this [pre training model.](http://data.airc.aist.go.jp/MUSIC4P3dataset/newmegasolarCNN_NS_72720_3725_iteration.model)

## Acknowledgement
This dataset and source code are based on results obtained from a project commissioned by the New Energy and Industrial Technology Development Organization (NEDO).  
