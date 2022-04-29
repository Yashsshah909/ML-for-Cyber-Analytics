# ML-for-Cyber-Analytics

Hi. 
In this repo, I have tackled a specific problem in the CIC IDS datasets for ml. There are a lot of fancy implementations of ML classification and deep learning algos out there for this and similar datasets, but none of them specifically consider some of the caetgorical features that it has. 
Hence I have looked at some of the categorical features in this and tried to use it for an ML model.

The dataset used in this is - IDS CIC 2017 dataset - https://www.unb.ca/cic/datasets/ids-2017.html. This link has a detailed explanation of what this dataset contains.

# Categorical features








# Pre-processing Data

After combining all the data from 5 days, we get about 2.8 million data. Also note there is a presence of garbage data in thursday morning data, hence I have not selected the whole dataset, but only the valid samples. 

We get a total of 14 labels/classes including benign and 13 types of attacks.

There are some columns/features which has only 1 value throught, this means there is no variance in this feature and hence it is useless for any ML model. I have dropped these columns. 

There are some ‘NAN’ and ‘infinite’ values in the features ‘Flow Bytes/s’ and ‘ Flow Packets/s’ which may be a problem when running any ML algo on the dataset.
The total number of samples which contain these are less than 0.01% of the total number of samples, hence I have decided to drop these samples completly. This won't affect the dataset much.

As for Scaling, I have decided to go with Robust Scaling, it uses the percentiles of 75 and 25 instead of max and min value in the formula - (value – median) / (p75 – p25). (where p=percentile). This method of scaling does not get affected by the outliers in the feature and at the same time keeps them in the dataset as it is. We do have quite a lot of outliers in some of these features, using a standard scaling or normalization method would lead to a massive disturbance in the distribution of the feature. 
