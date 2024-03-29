# Average of Averages
An average of average values may be required when aggregating data. There is a number of different average of average methods and they are differ to each other.<br>
Depending on an objective and a nature of data, one may choose one over others. <br>
Note that an average of averages does not necessarily equal to an average of a whole data, yet, it can provide a good estimation if the data is skewed, and
can help to present the data in different interpretations. 

## Table of Contents
- [Mean of Means](#mean-of-means)
- [Median of Medians](#median-of-medians)
- [Mean of Medians](#mean-of-medians)
- [Median of Means](#median-of-means)

## Mean of Means
A mean of means only equals to a mean of all when they number of values in aggregate groups are equal. <br>
When there is a skewness in numbers of values from the groups, the mean of means will differ to the mean of all.

ex) Class A has 10 students with grade 60 and class B has 1 student with grade 100.
- The mean of means will result in (60 + 100) / 2 = 80 <br>
- The mean of all will result in (10 * 60 + 1 * 100) / 11 = 63 <br>
Likewise, the mean of all will underpresent the student with grade 100. But the mean of means will make the underrepresented student with grade 100 stand out more.

## Median of Medians
A median of medians only equals to a median of all when the distribution of aggregate groups are equal. 

ex) Class A has grades of 10, 10, 20, 80 and class B has grades of 10, 60, 70 ,80, 80.
- The median of medians is median of (45, 45) = 45 <br>
- The median of all is median of (10, 80) = 45 <br>
Likewise, samples from the same distribution will have the median of medians equal to the median of all. 

One approach to make the median of medians close to the median of all is by:
- Taking a weighted median where the size of aggregate groups are taken into consideration. 

## Mean of Medians
A mean of medians is the best unbiased estimator* for the median.

*<i>Estimator: estimates statistical parameter such as mean</i>

## Median of Means
Note that an empirical mean is not a good central tendancy when data is heavy-tailed. <br>
A median of means (MOM) can be a good estimator of mean if the data distribution is heavily skewed (Sub-Gaussian). <br>
[Mean Estimation via MOM - mathematical proof](http://www.ub.edu/focm2017/slides/Lugosi.pdf)
