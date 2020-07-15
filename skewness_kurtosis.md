# Measure of Skewness and Kurtosis
Skewness and Kurtosis are used to describe the shapes of continuous distribution.

## Skewness
![](https://github.com/TravisH0301/learning/blob/master/images/skewness.png)
- Measure of symmetry around the mean of distribtuion
- Positive skew (skewed to left=long tail on right) where mode & median larger than mean
- Negative skew (skewed to right=long tail on left) where mode & median smaller than mean 
- Measure of skewness:
  - Karl Pearson's coefficient of skewness: Sk_p = (mean-mond)/sd
    <br>Median can be used instead of mode if mode is not unique in distribution: Sk_p = 3(mean-median)/sd
    <br>+ve value > postive skew | -ve value > negative skew
    
  - Bowley's coefficient of skewness: based on relative positions of median and quartiles
    <br>Sk_b = (Q3+Q1-2median)/(Q3-Q1)
    
  - Skewness value less than -1 or larger than +1: highly skewed
    <br>Skewness value in between [-1, -1/2] or [1/2, 1]: moderately skewed
    <br>Skewness value in between [-1/2, 1/2]: approximately symmetric

## Kurtosis
![](https://github.com/TravisH0301/learning/blob/master/images/kurtosis.jpg)
- Measure of tailedness of distribution (=flatness/peak of mode).
- Degree of concentration of observations around mode
- Measure of kurtosis:
  - Leptokurtic: highly peaked (B2 value > 3)

  - Mesokurtic: bell-curved peaked (B2 value = 3)

  - Platokurtic: flat-topped peaked (B2 value < 3)
