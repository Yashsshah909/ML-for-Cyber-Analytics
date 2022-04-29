# ML-for-Cyber-Analytics

Hi. 
In this repo, I have tackled a specific problem in the CIC IDS datasets for ml. There are a lot of fancy implementations of ML classification and deep learning algos out there for this and similar datasets, but none of them specifically consider some of the caetgorical features that it has. 
Hence I have looked at some of the categorical features in this and tried to use it for an ML model.

The dataset used in this is - IDS CIC 2017 dataset - https://www.unb.ca/cic/datasets/ids-2017.html. This link has a detailed explanation of what this dataset contains.

# Categorical features
Lets look at the categorical features we have with us - 
1. flow ID - It is just a combination of source IP, destination IP, source port, destination port and protocol. Hence it is not a real feature.

2. Source IP - The source IP address is normally the address that the packet was sent from. Attacks can come from any IP address and hence this feature did not seem reliable to use.

3. Destination IP - The IP address of the server to which traffic is forwarded. Same reason as source IP.

4. Destination Port - Destination Ports Are Server Applications. We have about 53805 unique dest. Ports. very high cardinality for a feature. 
Port number description - https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers.
Service Name and Transport Protocol Port Number Registry - https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml
Its datatype is integer and hence can be mistaken for a numerical feature. It is present in the MachineLearningCVE csv files and end up being used as a numerical feature.

5. Source Port - The source port is a next-available number assigned by TCP/IP to the user's machine. This assigned client number is how the network address translation (NAT), which typically resides in the router, determines which user to send back the responses to.

6. Protocol - A network protocol is a set of established rules that dictate how to format, transmit and receive data so that computer network devices - from servers and routers to endpoints - can communicate, regardless of the differences in their underlying infrastructures, designs or standards.

7. Timestamp - It is the part of the meta data and just denotes the timestamp of when the packet capture was done. hence it does not have much significance as a feature. 










# Pre-processing Data

After combining all the data from 5 days, we get about 2.8 million data. Also note there is a presence of garbage data in thursday morning data, hence I have not selected the whole dataset, but only the valid samples. 

We get a total of 14 labels/classes including benign and 13 types of attacks.

There are some columns/features which has only 1 value throught, this means there is no variance in this feature and hence it is useless for any ML model. I have dropped these columns. 

There are some ‘NAN’ and ‘infinite’ values in the features ‘Flow Bytes/s’ and ‘ Flow Packets/s’ which may be a problem when running any ML algo on the dataset.
The total number of samples which contain these are less than 0.01% of the total number of samples, hence I have decided to drop these samples completly. This won't affect the dataset much.

As for Scaling, I have decided to go with Robust Scaling, it uses the percentiles of 75 and 25 instead of max and min value in the formula - (value – median) / (p75 – p25). (where p=percentile). This method of scaling does not get affected by the outliers in the feature and at the same time keeps them in the dataset as it is. We do have quite a lot of outliers in some of these features, using a standard scaling or normalization method would lead to a massive disturbance in the distribution of the feature. 

# literature study
As for literature study I have refered to these 3 papers and some implementations of ml for this dataset on kaggle and github.

A. Two-Level Hybrid Model for Anomalous Activity Detection in IoT Networks - https://www.researchgate.net/publication/331417497_A_Two-Level_Hybrid_Model_for_Anomalous_Activity_Detection_in_IoT_Networks. 

Two level anomaly detection model - 1st model is flow based, 2nd model(random forest classifier) to find the category of anomaly.
For feature selection they have used Recursive Feature Elimination. (RFE)
For handling class imbalance they used Synthetic Minority Over-Sampling Technique (SMOTE) and Edited Nearest Neighbors(ENN) to create a synthetic sample of minority classes to improve the classification accuracy of minor classes.  
used the CICIDS2017 and UNSW-15 datasets and got good precision, recall and f-1 score for both.


B. Efficient Network Intrusion Detection Using PCA-Based Dimensionality Reduction of Features (CIC IDS 2017). - https://sci-hubtw.hkvisa.net/10.1109/isncc.2019.8909140#

PCA for dimension reduction (10 features).
IP (Internet Protocol) address to an integer representation.
Classifiers such as Random Forest (RF), Bayesian Network, Linear Discriminant Analysis (LDA) and Quadratic Discriminant Analysis (QDA). Training and testing sets with a ratio of 70:30.
Performance evaluation using Detection Rate (DR), F-Measure, False Alarm Rate (FAR), and Accuracy.
Compared their results with various other works.



C. Deep Learning for Cyber Security Intrusion Detection: Approaches, Datasets, and Comparative. - https://dora.dmu.ac.uk/bitstream/handle/2086/18792/Deep_Learning_for_Cyber_Security_Intrusion_Detection__Approaches__Datasets__and_Comparative_Study%20(1).pdf;jsessionid=7AF2EC244BD6F5477A2650C7E855BF67?sequence=1. 

A survey of various deep learning approaches for different types of cyber security ids datasets
Seven deep learning models including recurrent neural networks, deep neural networks, restricted Boltzmann machines, deep belief networks, convolutional neural networks, deep Boltzmann machines, and deep autoencoders.
Accuracy, false alarm rate, and detection rate for evaluating the efficiency
Worked on CSE-CICIDS2018 dataset and the Bot-IoT dataset.
