<h1><p align="center">Capstone Project on Data Science Nanodegree</p></h1>
<h1><p align="center">Food and Drug Prediction</p></h1>
<h3><p align="center">Israel Agwu Etu</p></h1>
<h3><p align="center">March 2022</p></h1>

## Introduction
<p align="justify">
This is the Capstone Project of the Udacity Nanodegree Program. In this project, there are different options to choose from, including embarking on a project of personal interest. I decided to embark on a project of personal interest. This project aims at developing a prediction algorithm for identifying foods, foodstuffs and drugs. I became interested in this work following my previous work on building a system that can identify foods and drugs. My interest in this field was triggered by the prevailing cases of food and drug adultration in our societies. I felt that it would be a good idea to develop a mobile, cheap and non-invasive device that can help consumers authenticate what they buy for consumption.<br>
The data used in this work were acquired live using an 18-channel sensor produced by Austriamicrosystems (AMS). These 18 channels were achieved by implementing semiconductor filters on the sensors which are capable of filtering from visible light to near Infrared bands.<br>
The technology behind this, is that of spectroscopy. Spectroscopy is the technique whereby electromagnetic waves interact with matter, with the intention of receiving reflected signals from the substance. These reflected signals carry the identity of the sample substance, commonly referred to as 'fingerprint'. These fiingerprints come in the form of lists of 18 values and it is expected that the fingerprint of a given substance under the influence of a given electromagnetic signal, will remain fairly the same. If there would be differences, it should not be too wide.<br>
Each substance is scanned a number of times and all the fingerprints saved in the database for onward training of the prediction model and final identification. It is common knowledge that the more the data about the substances, the more accurate the model will be in its prediction, hence each substance was scanned for 30 times.<br>
By the foregoing therefore, this work stems from a typical electrical and electronic engineering work where the data were acquired with a hardware device. However, I decided to import this hardware work into this all-software based work because of some challenges I observed while using typical sklearn algorithms to make predictions. These are the contents of the next section.
</p>

## Problem Statement
<p align="justify">
Going by the nature of data I was dealing with, my work actually has to do with supervised learning - all the datasets are labelled. For the fact that predictions have to be made by the system stuying the data patterns, sklearn calssification algorithms were the best options for me. As a result, Decision Tree and Random Forest classifiers were found more suitable. On one hand, decision tree exhibited inaccurate predition when the volume of data increased while the random forest demonstrated high-level consistency in prediction. On the other hand, both of them showed some behaviours which I consider not very satisfactory. It is these common behaviours that prompted me to go into this work and they include:<br>
  
- There is no visualization to show at least, the most probable substances that have fingerprints that are close to the one under analysis. Only one substance is predicted at a time given as output and the user has no idea how close the fingerprints of other substances in the database are to the one under study.<br>
- If a substance that is not yet included in the database is analyzed, these algorithms would just predict any of the existing substances. This gives a wrong impression.<br>
It is these two down sides that this works seeks to address and below is the procedure followed.
</p>

## Python Modules Used
The python modules used in this work include:<br>
- pandas<br>
- numpy<br>
- math<br>
- matplotlib<br>
- sklearn<br>

## Files in the Repository
The work was organized using the following files:

- **objdbase.csv:** This is the database file of the work. The data are saved in the csv format.

- **food_drug_predict.ipynb:** This file contains visualizations of the data and function-by-function implementation of the solution. It is a jupyter notebook file.

- **pred_class.ipynb:** This file contains the class implementation of the solution. It is also a jupyter notebook file.

## Methodology
<p align="justify">
It has already been mentioned that this is an integrated hardware plus software work that was imported into this typical software work. Therefore, the first step was to acquire the data and save them in the database using the hardware arrangement shown below.<br>

![fig1](https://user-images.githubusercontent.com/44449730/157485769-8a274b54-8bd6-4bd4-a57b-ea710872e491.JPG)<br>

The substances whose fingerprints were acquired includes some foods, foodstuffs and a drug, as will be shown later.<br>
With the data, a number of functions were created to manage the contents of the data such that the expected results could be achieved as closely as possible. The functions that do the work are as follows:<br>

- **val_ave():** This function performs element-wise addition and averaging of the numerical values of the datasets, substance-by-substance. At the end it produces a dataset of these values together with their corresponding substances.<br>
It takes a dataframe of fingerprints (numerical values) with their corresponding substance labels. One important condition is that all the substances must be scanned for equal number of times using the hardware as can be seen below.<br>
  
![fig2](https://user-images.githubusercontent.com/44449730/157499258-6a50f328-6e59-432c-9a3f-7aedf478ec37.JPG)<br>

Its return value is a list of lists of the averaged datasets, with their corresponding substance labels as can be seen below.<br>
 
![fig3](https://user-images.githubusercontent.com/44449730/157499401-9fe04a5f-897c-4402-9229-4722f6aee860.JPG)

- **list_diffs():** This function takes the element-wise differences between any new dataset of a substance and all the existing averaged datasets from the database. The intention is to see how close these differences are to zero. If the new dataset is exactly of equal values with any dataset in the list of lists, then the resulting difference dataset would come down to zeros.<br>
Thereafter, a threshold of 100 was used to take the element-wise difference of all these values again. By so doing, the closer the values are to 100, the more likely that substance is to be predicted. Finally their absolute values were taken.<br>
**Note: The value of the threshold can be changed at will.**<br>
It takes:<br> 
- obj_name: A dataset of a substance acquired in the form of a list of numerical values. Note that the length of this list must be equal with the number of columns of the database, excluding the substance name.<br>
For example: honey = [70, 141, 101, 155, 1514, 3401, 189, 270, 1564, 261, 262, 157, 186, 255, 244, 163, 2468, 673]<br>
- vals_lists: The output of the 'val_ave' function, shown above. This is usually a constant parameter.<br>
Its return value is a list of lists containing the absolute values of the element-wise differences between the new object dataset and the existing datasets from the 'val_ave' function; and again between the threshold value and the resultant datasets. See below<br>

![fig4](https://user-images.githubusercontent.com/44449730/157500350-5448bfd6-40e6-4544-be4e-da5723114155.JPG)<br>

- **pairing():** This function takes the average of each list in the resultant list of lists from the 'list_diffs' function. Each of these average values is paired with its corresponding name label. The idea is that the most likely substance would have an average value very close to the threshold (100 in this case).<br>
It takes the output of the 'list_diffs' function as a constant parameter (shown above) and returns a list of pairs of lists containing the average values and their corresponding object name labels. See below.<br>
 
![fig5](https://user-images.githubusercontent.com/44449730/157500834-3dbfa017-3940-4d8b-a4d8-5f313849764a.JPG)<br>

- **collections():** This function carries out the prediction proper, by comparing the individual average values with a range, set with 'min' and 'max' variables.<br> 
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

This is how the work was basically done; but to run the program, the procedure below may be followed.
</p>

## How To Run the Program
<p align="justify">
As mentioned earlier, the work is organized in two formats - functions and class. Their applications are almost the same except that in the function implementation a user interacts directly with the invidual functions while in the class implementation the user creates an instance of the class and thereafter accesses the functions indirectly. Moreover, while using the class implementation, the user inputs all the necessary parameters to the class only once and subsequently call the functions to view their outputs, if necessary. Therefore the class implementation is more compact and would be used in this demonstration.

- **Create an instance of the class**<br>

```preds = Predicts()```

- **Input necessary parameters**<br>

```preds = Predicts('objdbase.csv', milk3, min=50, max=280)```
  
where milk3 is a substance fingerprint: milk3 = [129, 235, 178, 299, 3593, 7257, 434, 516, 15344, 605, 1823, 327, 351, 266, 366, 307, 5096, 1311]
  
- **View outputs of the individual functions**<br>
- **val_ave function:**<br>
  
```
preds_val = preds.val_ave()
print(preds_val)
```

And below is the output - **a list of lists of the averaged datasets, with their corresponding substance labels**:<br>

![image](https://user-images.githubusercontent.com/44449730/157648215-5acd6365-36df-431d-9e52-2c168cb4bef3.png)

- **list_diffs function:**<br>
  
```
preds_list = preds.list_diffs()
print(preds_list)
```
  
And below is the output - **a list of lists containing the absolute values of the element-wise differences between the new object dataset and the existing datasets from the 'val_ave' function; and again between the threshold value and the resultant datasets**:<br>
  
![image](https://user-images.githubusercontent.com/44449730/157649284-d09fd8f9-a709-401f-889a-6bc076e36b3f.png)

Notice that most of the elements of the fingerprint of '3.5% Fat Milk' are now very close to the threshold value of 100.<br>
  
- **pairing function:**<br>
  
```
preds_pair = preds.pairing()
print(preds_pair)
```
  
And below is the output - **a list of pairs of lists containing the average values and their corresponding object name labels**:<br>
  
![image](https://user-images.githubusercontent.com/44449730/157650817-c111a22a-204a-4f37-81da-9f6e72f25202.png)

Notice that the value of '3.5% Fat Milk' is just a little above the threshold value of 100 (precisely 102).<br>
  
- **collections function:**<br>
This function returns two values and so is unpacked into two variables as can be seen below.<br>

```
preds_collect1, preds_collect2 = preds.collections()
print(preds_collect1)
print(preds_collect2)
```

'preds_collect1' contains more likely substance(s) while 'preds_collect2' contains less likely substances.<br>
And below are the outputs:<br> 
**More likely substance(s):**<br>
![image](https://user-images.githubusercontent.com/44449730/157652334-026f6273-46aa-4768-b96e-e0d239d5d160.png)

**Plot of more likely substance(s):**<br>
![image](https://user-images.githubusercontent.com/44449730/157652626-bdc22846-f0eb-4108-862e-64ac6a287a43.png)

**Less likely substance(s):**<br>
![image](https://user-images.githubusercontent.com/44449730/157652752-e63a6e9b-cd7b-47c1-a3ee-246d9abdf602.png)

**Plot of less likely substance(s):**<br>
![image](https://user-images.githubusercontent.com/44449730/157653612-fa78d75b-9163-45e1-bf48-4feae10d9cb9.png)

More about using this class implementation is that if a user inputs the wrong formats of parameters, it will throw an error message. For instance:<br>
- The fingerprint must be a list of 18 int elements.<br>
- The 'min' or 'max' value must not be negative.<br>
- The 'min' value must not be greater than the 'max vlaue'.<br>
There is an independent function called **'rand_forest'**. This function implements the random forest prediction approach. Just like the class version of this work, it also takes a dataframe of fingerprints of substances together with their substance name labels. It returns a name of a predicted substance. This function will be used to compare with the algorithm of this work to see their peculiarities.<br>
The next section tries to compare and contrast this work with the said sklearn algorithm.
</p>

## Comparison With Sklearn Algorithms
<p align="justify">
Using the Random Forest Classifier of the sklearn to predict the likely substance that has the fingerprint:
  
[129, 235, 176, 294, 3498, 7143, 437, 519, 15898, 612, 1892, 328, 356, 272, 370, 313, 5093, 1323]

the below output was gotten:

![image](https://user-images.githubusercontent.com/44449730/157667665-7a7f5bb6-e1e7-481e-8e75-57593944a027.png)

With the algorithm of this work the below result was also gotten:

![image](https://user-images.githubusercontent.com/44449730/157667960-448e1f4d-df67-473c-beea-899196540471.png)<br>
**Both outputs are correct predictions.**
  
In real life, due to the variations in the electrical properties of the hardware acquiring these fingerprints, some values of the fingerprints can vary. A typical example is what we have below, which is also a fingerprint of '3.5% Fat Milk':

[129, 235, 178, 299, 3593, 7257, 434, 516, 15344, 605, 1823, 327, 351, 266, 366, 307, 5096, 1311]

When analyzed:<br>
Random forest returned:

![image](https://user-images.githubusercontent.com/44449730/157669134-8ee03d2f-f51b-4239-abd8-e9340ea6ce4a.png)

This algorithm returned:

![image](https://user-images.githubusercontent.com/44449730/157669328-26b08803-7d08-4d7e-abb1-ce125144e5fc.png)

![image](https://user-images.githubusercontent.com/44449730/157669427-1b602c50-ce3a-4771-abd3-a9768414518d.png)

**Both predictions are again correct but notice that this algorithm was able to show another substance that is close to the predicted substance.** That is another probable substance but because the value is very far from the threshold (that is, 100), it is not the first choice. Moreover, other substances are also presented with their individual values to enable a user see how far they are from the threshold and the predicted substance. These visualizations are lacking in the random forest implementation.

In some other situations, a user may try to analyze a substance that does not exist in the database of the system. A typical case is the fingerprint below:

[231, 336, 278, 398, 3655, 7310, 547, 626, 16411, 725, 2033, 433, 463, 378, 477, 419, 5201, 1419]

Random forest returned:

![image](https://user-images.githubusercontent.com/44449730/157672659-61e76d24-bef3-4d8e-b7e6-cc9084c82c17.png)

This algorithm returned:

![image](https://user-images.githubusercontent.com/44449730/157672789-b5927195-3cd9-4822-b060-0298f9cc1be1.png)<br>
![image](https://user-images.githubusercontent.com/44449730/157672940-ca554a74-bf1a-4f5b-bf19-9643d5951af9.png)<br>
![image](https://user-images.githubusercontent.com/44449730/157673071-f33098c7-cd39-4f3b-a98e-daa68725f8ab.png)

The prediction result of the random forest algorithm is wrong in this case.<br> 
Check out the fingerprint below:

[131, 236, 178, 298, 3555, 7210, 447, 526, 16311, 625, 1933, 333, 363, 278, 377, 319, 5101, 1319]

It is one of the fingerprints of '3.5% Fat Milk'. If you add 100 to each of the elements, you get the unknown fingerprint used in the test above. You can see it again below.<br>

[231, 336, 278, 398, 3655, 7310, 547, 626, 16411, 725, 2033, 433, 463, 378, 477, 419, 5201, 1419]

Therefore it is so far an unknown substance, until it is included in the database with its label. What I expected the random forest algorithm to do is to give an indication that the fingerprint does not match any of them in the database; but it could not.<br>
Conversely, this algorithm could give a fair idea of what went on during the classification and eventual prediction. These visualizations are the core objectives of this project.<br>
</p>

## Summary
<p align="justify">
From the foregoing, the behaviours of the two different algorithms have been demonstrated. The random forest predicts very well, more especially if the test fingerprints are very similar to the ones in the database but does not give an indication about an unknown substance. Again, it does not give an idea of what goes on during the classification and eventual prediction of the test substance.<br>
On the other hand, this algorithm can present a fair idea of what goes on during the classification and prediction processes. It also shows other substances that are chemically related to the predicted substance. Moreover, it is capable of identifying an unknown suubstance when tested. Meanwhile this property depends on the values of 'min' and 'max'.
</p>

## Conclusion
<p align="justify">
It is good to point out that this work is not presented as a perfect work already. It is just an implementation of a conceptual idea which can still be worked upon for possible improvements. It is my belief that large packages such as the sklearn started this small. Therefore, any contribution that can improve the algorithm is highly welcome. So far, the algorithm does not split datasets into train and test components. In other words, it uses the entire datasets for training. It also does not fit separately like in the case of random forest. It simply goes ahead to make predictions. Furthermore, it does not calculate accuracy or percentage error. These are considered as further works that can be added in future; but for the purpose of this project which is time-bound, the work can safely be said to be sufficient at this point. On the whole, it has achieved the conceived concept and performs favorably too.<br>
For a blogpost version of this work, please visit https://medium.com/@israeletu/capstone-project-on-data-science-nanodegree-food-and-drug-prediction-e5e5054a17d9
</p>

## Acknowledgements
<p align="justify">
I thank God so much for enabling me to record this success in this program. It has been long-awaited. It was even further lengthened by the tragic breakdown of my laptop, in which case I lost many of my files including the first version of this work. It also came with a cost because I had to pay extra $100 for another one-month subscription to enable me finish the program. In all things I thank God.<br>
I thank the Udacity team for being very responsive with friendly listening ears. I am ever grateful.<br>
My appreciation also goes to all the sources I got inspiration and information that enabled me to complete this assignment. The sklearn documentation was very helpful, as well as the numpy documentation, among others.<br>
I would like to end by thanking Mosh Hamedani. He is my first python teacher and what I learnt from him gave me a good footing to cope with this data science program. My sincere gratitude goes to all of you my dear teachers.
</p>

## References
1. G. S. Bumbrah and R. M. Sharma, “Raman spectroscopy – Basic principle, instrumentation and selected applications for the characterization of drugs of abuse,” Egypt. J. Forensic Sci., vol. 6, no. 3, pp. 209–215, 2016.
2. S. Developers, “Sk-Learn,” 2019.
3. https://classroom.udacity.com/nanodegrees/nd025/parts/059c574b-e0d0-4fa7-8acd-47d9df9d53b6/modules/b0daab3f-5ffc-4ce1-af45-0945e87321ad/lessons/7420189a-47a9-43ad-9154-c9c0b98fdce5/concepts/a812e8b0-031e-463d-b5db-999a635a340e
4. https://numpy.org/doc/stable/reference/generated/numpy.add.html
5. https://docs.python.org/3/tutorial/errors.html
6. https://codewithmosh.com/courses/enrolled/417695
