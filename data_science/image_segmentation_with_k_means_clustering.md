# Image segmentation by K-Means Clustering 

## Table of Contents
- [K-means Clustering](#k-means-clustering)
  - [Optimal K value - Elbow method](#optimal-k-value---elbow-method)
- [Image segmentation with K-means Clustering](#image-segmentation-with-k-means-clustering)

Image segmentation is a process of grouping pixels into number of super pixels (group of pixels that share similar characteristics). 
This separates dictinct regions with similar attributes that helps to locate objects and boundaries in images. It is widely used in 
machine learning where the models can learn the images with more distinct objects. For example, below is the image of a lung CT scan 
that has gone through K-means Clustering. Likewise it removes noises in the image and makes the boundary and area of the lung more obvious.

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/k_means_img_seg1.png" width="400">
</p>

## K-means Clustering
K-means Clustering works by placing k number of clusters between data points and determine k number of groups based on the distance between
data points and clusters. It takes an initialisation of cluster centroid positions and iteration approach to determine the optimised cluster positions.
The following objective function is used and steps are shown below:

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/k_means_img_seg2.png" width="400">
</p>

1. Initialise k number of centroids
2. Assign data points to the nearest centroids
3. Calculate average of the data points in the clusters
4. Move the centroids to the calculated average position
5. Reassign data points to the nearest centroids
6. Repeat from 2 to 5 until it converges

The whole process in 2D is depicted below:

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/k_means_img_seg3.gif" width="400">
</p>

### Optimal K value - Elbow method
Finding the optimal K value can be ambiguous as there are no groud truth labels. One of the evaluation methods is Elbow method. This method evaluates K value
with total Within Cluster Sum of Squares (WCSS). WCSS is a measure of compactness of a cluster. It is calculated by summing up Euclidean distances between data points 
and the centroid of a cluster and diving the sum by the number of data points. And this WCSS for all clusters are added up for total WCSS. 

## Image segmentation with K-means Clustering
Implementation of K-means Clustering for image segmenation can be seen in the following notebook. In this notebook, Open CV (CV2) is used to process image and perform 
K-means Clustering. 

https://www.kaggle.com/travishong/covid-19-lung-ct-segmentation-classification
