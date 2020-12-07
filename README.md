# Chest X-Ray Classification

## Repository Structure

```
├── README.md                           <- The high-level overview of this project
├── chestxraypresentation.pdf           <- PDF version of project presentation
├── data                                <- Sourced externally
├── images                              <- Both sourced externally and generated from code
├── notebooks                           <- All notebooks used in this project, including the Final_Notebook and models notebook
├── presentation                        <- Contains final presentation
└── src                                 <- Code files referenced in project
```

## Business Problem

There are about a million adults in the US that have to be admitted to the hospital for pneumonia. Of those, 50,000 may die in year. One of the tests medical professionals can conduct to test for pneumonia is a chest x-ray scan. By creating a classification model that can predict if a x-ray indicates pneumonia or not, we can help medical professionals make a diagnosis for the patient faster and get to treatment more quickly, thus possibly saving lives.

## Exploratory Data Analysis

In this project we will be building an image classification model that classifies chest x-ray images as `normal` or `pneumonia`.  The model is trained on over 5000 images, from D. Kermany et al DOI:10.17632/rscbjbr9sj.3.

The images in this dataset were split into a training set, a validation set, and a test set. In the training set, there is a large class imbalance with 1299 `normal` x-ray images and 3833 x-ray images with `pnuemonia`. The validation set has no class imbalance with 50 `normal` x-ray images and 50 x-ray images with `pnuemonia`. The test set has a small imbalance with 234 `normal` x-ray images and 390 x-ray images with `pnuemonia`.

![](./images/chestxrays.png)

Visualizing one image from each class and set. From the images, we can tell there is a difference between the `normal` and `pnuemonia` x-ray images. The `pnuemonia` images are generally fuzzy or less clear in the lung regions. When radiologists look at chest x-ray image, one of the indicators of `pnuemonia` that they look for is the "ground-glass opacities". It describes the different shades of grey that can be found in between an normal chest x-ray and a chest x-ray of a patient with pnuemonia. As in the cases above, the `normal` images show clear lungs while in the `pnuemonia` images, there is an opacity in certain areas of the lung. In addition to opacities, there may be other features such as white spots, abcesses, fluid cavities, etc which may be indicative of pneumonia which the model may identify and use to classify the x-ray.

## Modelling

Our final model uses a convolutional neural net (CNN). In the context of image classification, CNN tend to perform better. The final model has 3 2D neural nets and 2 fully connected layers  with `relu` activation. The output layer is a single dense binary output with a `sigmoid` activation. For model diagnostics, we measured accuracy, precision, recall, and the AUC score. Due to the medical application of this model, the most important metric is the recall, because we want to minimize false negatives (we would rather predict a healthy patient has pneumonia than predict someone with pneumonia as healthy, and followup bloodwork can confirm this result).

![](./images/model.png)

## Results

With the final iteration of the model, a recall score of 0.99 was achieved, predicting nearly all of the true positives for chest x-rays with pneumonia.  The accuracy was 0.89, with a precision of 0.85.

Test loss: 0.4935239553451538

Test accuracy: 0.8894230723381042

Test precision: 0.8543046116828918

Test recall: 0.9923076629638672

Test AUC: 0.9413378834724426

![](./images/confmatrix.png)

## Conclusions and Future Steps

With a recall score of 0.99, almost all of the true positive cases are detected.  By adjusting the threshhold for the classifier, the results can be tuned for other desired metrics as well.  Adjusting the threshhold from 0.5 to 0.6 does not increase the number of false negatives but reduces the number of false positives.  This model performs well as net to catch potential infections that a radiologist might miss on visual inspection.

Some of the limitations of this model are that certain non-pneumonia features may resemble pneumonia features in a chest x-ray. According to C. Gibson et al., lung scaring and congenital heart failure have similar features to pneumonia in an x-ray image, which would result in false positives.  Additionally, pneumonia may occupy regions of the lung not visible in the x-ray.  They also claim that chest x-rays are nonspecific when diagnosing viral pneumonia, which would result in false negatives for viral pneumonia.

Future improvements to the model could be potential random transformations to the images such as vertical flip, rotations, zoom, and shift, to eliminate sensitivity to differences in the field of view of the image.  

Additionally, this model downscales images to 64 x 64 pixels, reducing the amount of detail in an image to increase computation speed.  A model that trains on higher resolution images will potentially see features that require more definition.  
