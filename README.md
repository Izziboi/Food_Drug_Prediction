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
The work was organized using the following files:<br><br>
**- objdbase.csv:** This is the database file of the work. The data are saved in the csv format.


**- food_drug_predict.ipynb:** This file contains visualizations of the data and function-by-function implementation of the solution. It is a jupyter notebook file.


**- pred_class.ipynb:** This file contains the class implementation of the solution. It is also a jupyter notebook file.

## Methodology
<p align="justify">It has already been mentioned that thhis is an integrated hardware plus software work that was imported into this typical software work. Therefore, the first step was to acquire the data and save them in the database using the hardware arrangement shown below.<br>

![fig1](https://user-images.githubusercontent.com/44449730/157485769-8a274b54-8bd6-4bd4-a57b-ea710872e491.JPG)<br>

The substances whose fingerprints were acquired includes some foods, foodstuffs and a drug, as will be shown later.<br>
With the data, a number of functions were created to manage the contents of the data such that the expected results could be achieved as closely as possible. The functions that do the work are as follows:<br>

**- val_ave():** This function performs element-wise addition and averaging of the numerical values of the datasets, substance-by-substance. At the end it produces a dataset of these values together with their corresponding substances.<br>
It takes a dataframe of fingerprints (numerical values) with their corresponding substance labels. One important condition is that all the substances must be scanned for equal number of times using the hardware as can be seen below.<br>
  
![fig2](https://user-images.githubusercontent.com/44449730/157499258-6a50f328-6e59-432c-9a3f-7aedf478ec37.JPG)<br>

Its return value is a list of lists of the averaged datasets, with their corresponding substance labels as can be seen below.<br>
 
![fig3](https://user-images.githubusercontent.com/44449730/157499401-9fe04a5f-897c-4402-9229-4722f6aee860.JPG)

**- list_diffs():** This function takes the element-wise differences between any new dataset of a substance and all the existing averaged datasets from the database. The intention is to see how close these differences are to zero. If the new dataset is exactly of equal values with any dataset in the list of lists, then the resulting difference dataset would come down to zeros.<br>
Thereafter, a threshold of 100 was used to take the element-wise difference of all these values again. By so doing, the closer the values are to 100, the more likely that substance is to be predicted. Finally their absolute values were taken.<br>
**Note: The value of the threshold can be changed at will.**<br>
It takes:<br> 
- obj_name: A dataset of a substance acquired in the form of a list of numerical values. Note that the length of this list must be equal with the number of columns of the database, excluding the substance name.<br>
For example: honey = [70, 141, 101, 155, 1514, 3401, 189, 270, 1564, 261, 262, 157, 186, 255, 244, 163, 2468, 673]<br>
- vals_lists: The output of the 'val_ave' function, shown above. This is usually a constant parameter.<br>
Its return value is a list of lists containing the absolute values of the element-wise differences between the new object dataset and the existing datasets from the 'val_ave' function; and again between the threshold value and the resultant datasets. See below<br>

![fig4](https://user-images.githubusercontent.com/44449730/157500350-5448bfd6-40e6-4544-be4e-da5723114155.JPG)<br>

**- pairing():** This function takes the average of each list in the resultant list of lists from the 'list_diffs' function. Each of these average values is paired with its corresponding name label. The idea is that the most likely substance would have an average value very close to the threshold (100 in this case).<br>
It takes the output of the 'list_diffs' function as a constant parameter (shown above) and returns a list of pairs of lists containing the average values and their corresponding object name labels. See below.<br>
 
![fig5](https://user-images.githubusercontent.com/44449730/157500834-3dbfa017-3940-4d8b-a4d8-5f313849764a.JPG)<br>

**- collections():** This function carries out the prediction proper, by comparing the individual average values with a range, set with 'min' and 'max' variables.<br> 
**Note: The value of these variables can be changed at will.** They determine how many substances that can be suggested as the likely matching substances each time a prediction is made.<br>
This function takes 'div_lists' as a constant parameter. It is the output of the 'pairing' function shown above.<br>
It returns the following:<br>
- A list of lists of the predicted substance(s) with their averaged values. See below.<br>
![fig6](https://user-images.githubusercontent.com/44449730/157501716-bb59e240-0935-4a96-8047-c3dab08f8267.JPG)<br>

- A list of lists of the unlikely substances with their averaged values. See below.
  
![fig7](https://user-images.githubusercontent.com/44449730/157501958-a15ca141-9ec1-4074-b851-d03400e42bf3.JPG)<br>

- A plot of the predicted substance(s) with their averaged values. See below.<br>
![fig8](https://user-images.githubusercontent.com/44449730/157502174-72ed0a84-31a2-4369-bb07-8ea81eaf4da0.JPG)<br>

- A plot of the unlikely substances with their averaged values. See below.<br>
![fig9](https://user-images.githubusercontent.com/44449730/157502332-73805582-e3df-403d-9d69-d71e08eabde4.JPG)<br>

</p>

## Comparison With Sklearn Algorithms
## How To Run the Program
## Summary (including downside, eg not able to split and fit, using all data to train. May be improved upon)
## Conclusion
## References
