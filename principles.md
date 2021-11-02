# Overfitting and Underfitting principles
## Understand basic principles of underfitting and overfitting and why you should use particular techniques to deal with them

A lot of articles have been written about overfitting, but almost all of them *are simply a list of tools*. "Top 10 tools to fight overfitting you should know" or "Best techniques to combat overfitting". *It's like being shown nails without explaining how to hammer them*. It can be very confusing for people who are trying to figure out how overfitting works. Also, these articles often do not consider underfitting, as if it does not exist at all.

In this article, I would like to list the **basic principles** (exactly *principles*) for improving the quality of your model and, accordingly, preventing underfitting and overfitting on a particular example. This is a very general issue that can apply to all algorithms and models, so it is very difficult to fully describe it. But I want to try to give you an *understanding* of why underfitting and overfitting occur and why one or another particular technique should be used.

## Underfitting and Overfitting and Bias/Variance Trade-off

Although I'm not describing all the concepts you need to know here (for example, *quality metrics* or *cross-validation*), I think it's important to explain to you (or just remind you) what underfitting/overfitting is.

To figure this out, let's create some dataset, split it into train and test sets, and then train three models on it - simple, good and complex (I will not use a validation set in this example to simplify it, but I will tell about it later). All code is available in this [GitLab repo](https://gitlab.com/Winston-90/underfitting_vs_overfitting).

| ![dataset.jpg](./img/dataset.jpg) |
|:--:|
| <b>Generated dataset. Image by Author</b>|

**Underfitting** is a situation when your model is **too simple** for your data. More formally, your hypothesis about data distribution is wrong and too simple - for example, your data is quadratic and your model is linear. 
This situation is also called **high bias**. This means that your algorithm can do accurate predictions, but the initial assumption about the data is incorrect.

| ![underfitting.jpg](./img/underfitting.jpg) |
|:--:|
| <b>Underfitting. The linear model trained on cubic data. Image by Author</b>|

Opposite, **overfitting** is a situation when your model is **too complex** for your data. More formally, your hypothesis about data distribution is wrong and too complex - for example, your data is linear and your model is high-degree polynomial.
This situation is also called **high variance**. This means that your algorithm can't do accurate predictions - changing the input data only a little, the model output changes very much.

| ![overfitting.jpg](./img/overfitting.jpg) |
|:--:|
| <b>Overfitting. The 13-degree polynomial model trained on cubic data. Image by Author</b>|

These are two extremes of the same problem and the optimal solution always lies somewhere in the middle.

| ![good_model.jpg](./img/good_model.jpg) |
|:--:|
| <b>Good model. The cubic model trained on cubic data. Image by Author</b>|

I will not talk much about bias/variance trade-off (you can read the basics [in this article](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229)), but let me briefly mention possible options:
- low bias, low variance - is a good result, just right.
- low bias, **high variance** - **overfitting** - the algorithm outputs very different predictions for similar data.
- **high bias**, low variance - **underfitting** - the algorithm outputs similar predictions for similar data, but predictions are wrong (algorithm "miss").
- high bias, high variance - very bad algorithm. You will most likely never see this.

| ![bias_variance_four_plots.jpg](./img/bias_variance_four_plots.jpg) |
|:--:|
| <b>Bias and Variance options on four plots. Image by Author</b>|

All these cases can be placed on the same plot. It is a bit less clear than the previous one but more compact.

| ![bias_variance_one_plot.jpg](./img/bias_variance_one_plot.jpg) |
|:--:|
| <b>Bias and Variance options on one plot. Image by Author</b>|

### How to Detect Underfitting and Overfitting

Before we move on to the tools, let's understand how to "diagnose" underfitting and overfitting.

| ![train_test_error.png](./img/train_test_error.png) |
|:--:|
| <b>Train/test error and underfitting/overfitting. Image by Author</b>|

**Underfitting** means that your model makes accurate, but initially incorrect predictions. In this case, **train error is large** and **val/test error is large** too.

**Overfitting** means that your model makes not accurate predictions. In this case, **train error is very small** and **val/test error is large**.

When you find a **good model**, **train error is small** (but larger than in the case of overfitting) and **val/test error is small** too.

In the case above, the test error and validation error are approximately the same. This happens when everything is fine, and your *train, validation, and test data have the same distributions*. If validation and test error are very different, then you need to get more data similar to test data and make sure that you split the data correctly.

| ![detect_underfitting_overfitting.png](./img/detect_underfitting_overfitting.png) |
|:--:|
| <b>How to detect underfitting and overfitting. Image by Author</b>|

## Tools and Techniques

Now let's look at techniques to prevent underfitting and overfitting, considering exactly *why we should use them*.

### General Intuition You Should Remember

As we remember:
- **underfitting** occurs when your model is **too simple** for your data.
- **overfitting** occurs when your model is **too complex** for your data.

Based on this, simple intuition you should keep in mind is:
- to fix **underfitting**, you should **complicate** the model.
- to fix **overfitting**, you should **simplify** the model.

In fact, everything that will be listed below is only the consequence of this simple rule. I will try to show why certain actions will complicate or simplify the model.

### More Simple / Complex Model

The easiest way that comes to mind based on the intuition above is to try a more simple or more complex algorithm (model).

To complicate the model, you need to add more parameters (*degrees of freedom*). Sometimes this means to directly try a more powerful model - one that is a priori capable to restore more complex dependencies (*SVM with different kernels instead of logistic regression*). **If the algorithm is already quite complex** (neural network or some ensemble model), **you need to add more parameters** to it, for example, increase the number of models in boosting. In the context of neural networks, this means adding more layers / more neurons in each layer / more connections between layers / more filters for CNN, and so on.

To simplify the model, you need contrariwise to reduce the number of parameters. Either completely change the algorithm (*try random forest instead of deep neural network*), or reduce the number of degrees of freedom. Fewer layers, fewer neurons, and so on.

### More Regularization / Less Regularization

This point is very closely related to the previous one. In fact, **regularization is an indirect and forced simplification of the model**. The regularization term requires the model to keep parameters values as small as possible, so requires the model to be as simple as possible. Complex models with strong regularization often perform better than initially simple models, so this is a very powerful tool.

| ![regularization.jpg](./img/regularization.jpg) |
|:--:|
| <b>Good model and complex model with regularization. Image by Author</b>|

More regularization (simplifying the model) means increasing the impact of the *regularization term*. This process is strictly individual - depending on the algorithm, the regularization parameters are different (for example, **to reduce the regularization, the alpha for Ridge regression should be decreased, and C for SVM - increased**). So you should study the parameters of the algorithm and pay attention to whether they should be increased or decreased in a particular situation. There are a lot of such parameters - L1/L2 coefficients for linear regression, C and gamma for SVM, maximum tree depth for decision trees, and so on. In the context of neural networks, the main regularization methods are:
- Early stopping,
- Dropout,
- L1 and L2 Regularization.

You can read about them [in this article](https://medium.com/@ODSC/classic-regularization-techniques-in-neural-networks-68bccee03764).

Opposite, in the case when the model needs to be complicated, you should reduce the influence of regularization terms or abandon the regularization at all and see what happens.

### More Features / Less Features

This may not be so obvious, but adding new features also complicates the model. Think about it in the context of a *polynomial regression* - adding quadratic features to a dataset allows a linear model to recover quadratic data.

*Adding new "natural" features* (if you can call it that) - obtaining new features for existing data is used infrequently, mainly due to the fact that it is very expensive and long. But you can keep in mind that sometimes this can help.

But artificial obtaining of additional features from existing ones (the so-called *feature engineering*) is used quite often for classical machine learning models. There are as many examples of such transformations as you can imagine, but here are the main ones:
- polynomial features - from *x1, x2* to *x1, x2, x1x2, x1^2, x2^2* (`sklearn.preprocessing.PolynomialFeatures` class)
- log(x), for data with not-normal distribution
- ln(|x| + 1) for data with heavy right tail
- transformation of categorical features 
- other non-linear data transformation (from length and width to area (length*width)) and so on.

If you need to simplify the model, then you should use a smaller quantity of features. First of all, remove all the additional features that you added earlier if you did so. But it may turn out that in the original dataset there are features that do not carry useful information, and sometimes cause problems. Linear models often work worse if some features are dependent - *highly correlated*. In this case, you need to use *feature selection* approaches to select only those features that carry the maximum amount of useful information.

It is worthwhile to say that in the context of neural networks, *feature engineering* and *feature selection* make almost no sense because **the network finds dependencies in the data itself**. This is actually why deep neural networks can restore such complex dependencies.

### Why Getting More Data Sometimes Can't Help

One of the techniques to combat overfitting is to get more data. However, surprisingly, this may not always help. Let's generate a similar dataset 10 times larger and train the same models on it.

| ![more_data.jpg](./img/more_data.jpg) |
|:--:|
| <b>Why getting more data sometimes can't help. Image by Author</b>|

A very simple model (degree 1) has remained simple, almost nothing has changed. So **getting more data will not help in case of underfitting**.

But the complex model (degree 13) has changed for the better. It is still worse than the initially good model (degree 3), but much better than the original one. Why did this happen?

Last time (for the initial dataset), the model was trained on 14 data points (*20 (initial dataset size) * 0.7 (train ratio) = 14*). A 13-degree polynomial can perfectly match these data (by analogy, we can draw an ideal straight line (degree=1) through 2 points, an ideal parabola (degree=2) through 3 points, and so on). By getting 10 times more data, the size of our train set is now 140 data points. To perfectly match these data, we need a 139-degree polynomial!

Note, that if we had initially trained a *VERY complex model* (for example, 150-degree polynomial), such an increase in data would not have helped. So get more data is a good way to improve the quality of the model, but it may not help if the model is very very complex.

So, the conclusion is - **getting more data can help only with overfitting (not underfitting) and if your model is not TOO complex**. 

In the context of computer vision, getting more data can also mean *[data augmentation](https://en.wikipedia.org/wiki/Data_augmentation)*. 

## Summary

Let's summarize everything in one table. 

| ![summary_table.png](./img/summary_table.png) |
|:--:|
| <b>Techniques to fight underfitting and overfitting. Image by Author</b>|

Well, better in two.

| ![summary_table_extended.png](./img/summary_table_extended.png) |
|:--:|
| <b>Techniques to fight underfitting and overfitting (extended). Image by Author</b>|

Some tools and techniques have not been covered in this article. For example, I consider **data cleaning** and **cross-validation or hold-out validation** to be common practices in any machine learning project, but they can also be considered as tools to combat overfitting.

You may notice that to eliminate underfitting or overfitting, you need to apply **diametrically opposite actions**. So if you initially "misdiagnosed" your model, you can spend a lot of time and money on empty work (for example, getting new data when in fact you need to complicate the model). That's why it is so important - hours of analysis can save your days and weeks of work.

In addition to the usual analysis of the model quality (train/test errors), there are many techniques for understanding exactly what needs to be done to improve the model ([error analysis](https://www.analyticsvidhya.com/blog/2021/08/a-quick-guide-to-error-analysis-for-machine-learning-classification-models/), [ceiling analysis](https://www.coursera.org/lecture/machine-learning/ceiling-analysis-what-part-of-the-pipeline-to-work-on-next-LrJbq), etc.). Unfortunately, this topic is beyond the scope of this article. 

However, all these procedures have the purpose of understanding where to move and what to pay attention to. I hope this article helps you to understand the basic principles of underfitting and overfitting and motivate you to learn more about them.
