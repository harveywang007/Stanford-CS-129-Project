# Stanford-CS-129-Project

For Stanford's CS 129 course, Applied Machine Learning, I worked on a final project with two other classmates. These are my coding contributions.

In our project, we predicted depression based on RNA expression. The algorithms used included a neural network, logistic regression, SVM, and an anomaly detection algorithm; PCA was also tested, but it did not improve the performance. I wrote the anomaly detection and SVM algorithms, and I will share those here.

The data set was a proprietary set provided by one of the group members. While it does not include identifying data, I will not share it just to be safe. I have also cleared out the cell outputs that showed the data.

## The Dataset
The data had 98 columns corresponding to each gene, a column corresponding to the samples' indices, and a column with BDI scores. The 98 columns were the inputs and the BDI scores were the outputs. There were around only 120 samples, so the predictions are not accurate. The anomaly detection algorithm was the most successful, but only worked because of a shotgun approach.

## Anomaly Detection
Because of the large disparity between the amount of depressed and non-depressed cases in our data, we also tried an anomaly detection algorithm. Depressed cases were considered to be anomalies. Like previous algorithms, the data was split into 60% training examples, 20% cross-validation examples, and 20% test examples. However, the training set consisted of only non-depressed examples in order for the algorithm to learn the probabilities of normal cases. So, we could not apply the train_test_split method for the training examples. Instead, we ordered the dataset by ascending values based on the BDI score converted into binary. The data with 0.0-values were separated from the dataset and randomized. The split dataset was then reconstituted. The top 60% were taken to form the training dataset; the remaining 40% was randomized through train_test_split. The cross-validation set had 16 0.0-values and 9 1.0-values; the test set had 8 0.0-values and 17-1.0 values. As can be noted, the cross-validation and test sets are uneven.

The multivariate_normal method from the SciPy library was used to model the Gaussian distribution. The threshold was chosen by training on the cross-validation set. Probability values from the minimum value to the maximum value were tried, in increments of the range divided by 1000. The threshold with the best F1 score was chosen. The threshold was found to be 6.5e-49, with an F1 score of 0.48. This threshold value was applied on a test set. The precision score was 0.67; the recall score was 0.94; and the F1 score was 0.78. As can be seen in Figure 9, the algorithm is predicting all 1.0s, except for one input; the majority of the test data are 1.0s. This algorithm was successful because of a “shotgun” approach.

## SVM Algorithm
Using scikit-learn's SVM module, we trained an SVM model. We iterated over a list of C-values from 0.005 to 0.1, incremented by 0.005, to determine the optimal regularization parameter. In each iteration, the SVC function, with a linear kernel and a class weight of 50 for the data with a y-value of 1.0, and the fit function were used. The use of class weight also prevented divisions by zero when calculating the precision and recall; like in logistic regression, the algorithm predicted zeros because of our skewed data set. Precision and recall functions were used, and the F1 score was calculated from the precision and recall values. A C-value of 0.005 gave the highest scores. So, the algorithm with a C-value of 0.005 was used on the test set, giving a precision score of 0.29, a recall score of 0.83, and an F1 score of 0.43. Additionally, an ROC curve was made using the test data. The AUC was 0.64.
