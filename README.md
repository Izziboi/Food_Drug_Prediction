<h1><p align="center">Capstone Project on Data Science Nanodegree</p></h1>
<h1><p align="center">Food and Drug Prediction</p></h1>
<h3><p align="center">Israel Agwu Etu</p></h1>
<h3><p align="center">March 2022</p></h1>

## Introduction
<p align="justify">This is the Capstone Project of the Udacity Nanodegree Program. In this project, there are different options to choose from, including embarking on a project of personal interest. I decided to embark on a project of personal interest. This project aims at developing a prediction algorithm for identifying foods, foodstuffs and drugs. I became interested in this work following my previous work on building a system that can identify foods and drugs. My interest in this field was triggered by the prevailing cases of food and drug adultration in our societies. I felt that it would be a good idea to develop a mobile, cheap and non-invasive device that can help consumers authenticate what they buy for consumption.<br>
The data used in this work were acquired live using an 18-channel sensor produced by Austriamicrosystems (AMS). These 18 channels were achieved by implementing semiconductor filters on the sensors which are capable of filtering from visible light to near Infrared bands.<br>
The technology behind this, is that of spectroscopy. Spectroscopy is the technique whereby electromagnetic waves interact with matter, with the intention of receiving reflected signals from the substance. These reflected signals carry the identity of the sample substance, commonly referred to as 'fingerprint'. These fiingerprints come in the form of lists of 18 values and it is expected that the fingerprint of a given substance under the influence of a given electromagnetic signal, will remain fairly the same. If there would be differences, it should not be too wide.<br>
Each substance is scanned a number of times and all the fingerprints saved in the database for onward training of the prediction model and final identification. It is common knowledge that the more the data about the substances, the more accurate the model will be in its prediction, hence each substance was scanned for 30 times.<br>
By the foregoing therefore, this work stems from a typical electrical and electronic engineering work where the data were acquired with a hardware device. However, I decided to import this hardware work into this all-software based work because of some challenges I observed while using typical sklearn algorithms to make predictions. These are the contents of the next section.</p>

## Problem Statement
<p align="justify">Going by the nature of data I was dealing with, my work actually has to do with supervised learning - all the datasets are labelled. For the fact that predictions have to be made by the system stuying the data patterns, sklearn calssification algorithms were the best options for me. As a result, Decision Tree and Random Forest classifiers were found more suitable. On one hand, decision tree exhibited inaccurate predition when the volume of data increased while the random forest demonstrated high-level consistency in prediction. On the other hand, both of them showed some behaviours which I consider not very satisfactory. It is these common behaviours that prompted me to go into this work and they include:<br>
  
- There is no visualization to show at least, the most probable substances that have fingerprints that are close to the one under analysis. Only one substance is predicted at a time given as output and the user has no idea how close the fingerprints of other substances in the database are to the one under study.<br>
- If a substance that is not yet included in the database is analyzed, these algorithms would just predict any of the existing substances. This gives a wrong impression.<br>
It is these two down sides that this works seeks to address and below is the procedure followed.</p>

## Python Modules Used
The python modules used in this work include:<br>
- pandas<br>
- numpy<br>
- math<br>
- matplotlib<br>
- sklearn<br>

## Files in the Repository
The work was organized using the following files:<br>
**- objdbase.csv:** This is the database file of the work. The data are saved in the csv format.<br>
**- food_drug_predict.ipynb:** This file contains visualizations of the data and function-by-function implementation of the solution. It is a jupyter notebook file.<br>
**- pred_class.ipynb:** This file contains the class implementation of the solution. It is also a jupyter notebook file.<br>

## Methodology
<p align="justify">It has already been mentioned that thhis is an integrated hardware plus software work that was imported into this typical software work. Therefore, the first step was to acquire the data and save them in the database using the hardware arrangement shown below.<br>

![fig1](https://user-images.githubusercontent.com/44449730/157485769-8a274b54-8bd6-4bd4-a57b-ea710872e491.JPG)<br>

The substances whose fingerprints were acquired includes some foods, foodstuffs and a drug, as will be shown later.<br>
With the data, a number of functions were created to manage the contents of the data such that the expected results could be achieved as closely as possible. The functions that do the work are as follows:<br>

**- val_ave():** 
</p>
