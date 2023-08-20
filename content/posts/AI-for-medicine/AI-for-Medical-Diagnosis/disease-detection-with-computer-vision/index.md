---
title: "Disease Detection with Computer Vision"
author: ["KayPro"]
date: 2023-08-18T20:50:25+02:00
draft: false
hero: "images/hero.jpg"
module:
  imports:
  - path: github.com/hugomods/katex
menu:
  sidebar:
    parent: "ai-for-medical-diagnosis"
    weight: 20
    identifier: "disease-detection-with-computer-vision"
    name: "Disease-Detection-with-Computer-Vision"
---

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [Data Exploration and Image Processing](#data-exploration-and-image-processing)
    - [Navigating the Terrain of Data Exploration](#navigating-the-terrain-of-data-exploration)
    - [Image Processing Using Keras](#image-processing-using-keras)
    - [Data Preparation](#data-preparation)

</div>
<!--endtoc-->



## Data Exploration and Image Processing {#data-exploration-and-image-processing}

In this blog post, I'm excited to share my learnings in the course
"AI-for-medical-Diagnoses" about two fundamental aspects that play a crucial role in this journey: Data Exploration and Image Processing.


### Navigating the Terrain of Data Exploration {#navigating-the-terrain-of-data-exploration}

-   **Data Types and Null-Value Checks**:
    Before we start our journey, it's important to acquaint ourselves with the types of data we're dealing with. A thorough understanding of data types and a meticulous check for missing values lay the groundwork for accurate analysis.

-   **Unique IDs Check**
    To ensure data integrity, it's crucial to perform a unique IDs check, thereby avoiding any duplication of patient IDs between the train and test sets.

-   **Data Labels**
    When dealing with data labels, it's important to verify whether the dataset maintains a balanced distribution across different classes.

-   **Data Visualization**
    Engaging in data visualization involves randomly selecting images to immerse oneself in the dataset's nuances and gain an intuitive understanding of its characteristics.

-   **Investigate Single Images**
    When investigating a single image, it's essential to analyze its dimensions, identify the maximum pixel value, and calculate the mean value of the pixels to gain comprehensive insights.

-   **Investigating Pixel Distribution**
    To delve into the distribution of pixel values, it's insightful to create a
    histogram using seaborn, shedding light on the spread of pixel values across the image.
    ```python
    sns.distplot(                   # distribution plot
        raw_image.ravel(),          # flattening the image
        label=f"Pixel Mean {np.mean(raw_image):.4f} & Standard Deviation {np.std(raw_image):.4f}",
        kde=False,
    )
    ```


### Image Processing Using Keras {#image-processing-using-keras}

-   **Normalizing with ImageDataGenerator**
    The process of normalization through Keras' ImageDataGenerator involves adjusting pixel values to achieve a mean of 0 and a standard deviation of 1, as depicted by the formula

    <div class="katex">

    $$x_i = \frac{x_i - \mu}{\sigma}$$

    </div>

    ```python
    # Normalize images
    image_generator = ImageDataGenerator(
        samplewise_center=True, #Set each sample mean to 0.
        samplewise_std_normalization= True # Divide each input by its standard deviation
        )
    ```

-   **Processing the images with ImageDataGenerator**
    ```python
    # Flow from directory with specified batch size and target image size
    generator = image_generator.flow_from_dataframe(
            dataframe=train_df,
            directory="data/images-small/",
            x_col="Image", # features
            # Let's say we build a model for mass detection
            y_col= ['Mass'], # labels
            class_mode="raw", # 'Mass' column should be in train_df
            batch_size= 1, # images per batch
            shuffle=False, # shuffle the rows or not
            target_size=(320,320) # width and height of output image
    )
    ```


### Data Preparation {#data-preparation}


#### Problems {#problems}

During the process of model training, several key challenges emerge,
encompassing the management of **class imbalance**, the intricacies of **multi-task
learning**, and the profound influence of **dataset size**.

Tackling the skewed distribution of classes, optimizing models for multiple tasks, and effectively harnessing datasets of varying extents all stand as critical hurdles in this phase of machine learning advancement. Addressing these challenges with innovative techniques is pivotal to enhancing the efficiency and efficacy of trained models.


#### Class-Imbalance Problem {#class-imbalance-problem}

1.  **Resampling**
    In order to counter the effects of class imbalance, techniques such as resampling can be employed to ensure an equitable distribution of positive and negative labels, while also considering options like over- and undersampling, which involve selectively sampling from various groups, potentially involving duplication and exclusion of certain datapoints.
2.  **Counting Lables and Weighted Loss Function**
    To balance the contribution of both labels, a weighting approach can be implemented. This involves assigning weights to each label, ensuring equal influence. In this context, the weight for the positive label (w_p) is determined by dividing the number of negative samples by the total number of samples, while the weight for the negative label (w_n) is calculated by dividing the number of positive samples by the total number of samples.
    \\[ E = -J \sum\_{i=1}^N s\_i s\_{i+1} \\]
    \\( E = -J \sum\_{i=1}^N s\_i s\_{i+1} \\)

    \\[w\_p = \frac{num\\\_negative}{num\_total}\\]
    \\[w\_n = \frac{num\_positive}{num\_total}\\]

    <div class="latex">

    \begin{equation}
    \label{eq:1}
    C = W\log\_{2} (1+\mathrm{SNR})
    \end{equation}

    </div>

    w_n = \frac


#### Dataset Split {#dataset-split}

\\(a=+\sqrt{2}\\)
Raw Mathjax block:


$$a_4 \ne b_4$$
\\[a_4 \ne b_4\\]
