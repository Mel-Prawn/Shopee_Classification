## Project Overview

E-commerce sites such as Shopee receive multiple product listings daily. To improve recommendations for both retailers and customers, there is a need to identify listings which represent identical products.

Two different images of similar wares may represent the same product or two completely different items. Retailers want to avoid misrepresentations and other issues that could come from conflating two dissimilar products. Currently, a combination of deep learning and traditional machine learning analyzes image and text information to compare similarity. But major differences in images, titles, and product descriptions prevent these methods from being entirely effective.

Note: This project was adapted from a Kaggle competition. Data and full overview can be found in this [LINK](https://www.kaggle.com/competitions/shopee-product-matching/overview).

### Project Goal

This project aims to create a model that can determine which products are the same based on their product images. Model performance will be evaluated via mean F1 score.

---

### Methods
The issue presented falls under the similarity learning paradigm. We will first use image perceptual hash(phash) to establish a base model. Images with small phash differences will be considered as the same products.

Next we will use pre-trained Convolutionary Neural Networks(CNN) to convert the images into features. Images which are close together in the feature space will be grouped together and considered as the same products.

### Results

| **Model**                | **Epochs/ Metric** | **Best Threshold** | **Train Set**<br/> (Mean F1) | **Train Set**<br/> (Mean F1) |
|--------------------------|--------------------|--------------------|------------------------------|------------------------------|
| phash similarity         | -                  | 9                  | 0.596                        | 0.613                        |
| EnetB0_TL                | -                  | 7.5                | 0.649                        | 0.671                        |
| EnetB0_FT (1 layer)      | 3/ Euclidean       | 16.5               | 0.664                        | 0.686                        |
| EnetB0_FT (1 layer)      | 6/ Euclidean       | 18.25              | 0.664                        | 0.688                        |
| EnetB0_FT (1 Module)     | 3/ Euclidean       | 27.75              | 0.681                        | 0.696                        |
| EnetB0_FT (1 Module)     | 6/ Euclidean       | 32.25              | 0.686                        | 0.701                        |
| EnetB0_FT (1 Module)     | 9/ Euclidean       | 32.25              | 0.686                        | 0.701                        |
| **EnetB0_FT (1 Module)** | **6/ Cosine**      | **0.225**          | **0.716**                    | **0.724**                    |


The final model selected is EfficientNetB0 after fine tuning for 6 epochs with 1 module unfrozen. The Cosine distance metric was used to determine which are the same products.

---

#### Conclusions and Reccomendations

The model does well in predicting similar items with a mean F1 score of 0.716 on the train data and 0.724 on the test data. 

The False Positives can be improved further, but are not detrimental to customer experience as these products are still related to the product of interest (e.g. It may reccomend similar products which are not the same exact model). Nonetheless, the model falls short in taking into account the semantics of the product resulting in many false negatives (i.e. Identical products with very different images fail to be detected).

Predictions can be improved by including features which account for these semantics such as product title or product category.