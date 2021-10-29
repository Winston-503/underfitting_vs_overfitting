# Overfitting and Underfitting principles
## Understand what it is and why you should use particular techniques to prevent this

A lot of articles have been written about overfitting, but almost all of them *are simply a list of tools*. "How to handle overfitting - top 10 tools" or "best techniques to prevent overfitting". *It's like being shown nails without explaining how to hammer them*. It can be very confusing for people who are trying to figure out how overfitting works. Also, these articles often do not consider underfitting, as if it does not exist at all.

In this article, I would like to list the **basic principles** (exactly *principles*) for improving the quality of your model and, accordingly prevent underfitting and overfitting on a particular example. This is a very general issue that can apply to all algorithms and models, so it is very difficult to fully describe it. But I want to try to give you an *understanding* of why underfitting and overfitting occur and why one or another technique should be used.

## Underfitting and Overfitting and Bias/Variance Trade-off

Although I'm not describing all the concepts you need to know here (for example, *quality metrics* or *cross-validation*), I think it's important to explain to you what underfitting/overfitting is.

To figure this out, let's create some dataset, split it into train and test (I will not use a validation set in this example to simplify it, but I will tell about it later), and then train three models on it - simple, good and complex. All code is available in [gitlab repo](https://gitlab.com/Winston-90/underfitting_vs_overfitting).

| ![dataset.jpg](./img/dataset.jpg) |
|:--:|
| <b>Generated dataset. Image by Author</b>|

**Underfitting** is a situation when your model is **too simple** for your data. More formally, your hypothesis about data distribution is wrong and too simple - for example, your data is quadratic and your model is linear. 
This situation is also called **high bias**. This means that your algorithm can do accurate predictions, but the initial assumption about the data is incorrect.

| ![underfitting.jpg](./img/underfitting.jpg) |
|:--:|
| <b>Underfitting. The linear model is applied to cubic data. Image by Author</b>|

Opposite, **overfitting** is a situation when your model is **too complex** for your data. More formally, your hypothesis about data distribution is wrong and too complex - for example, your data is linear and your model is high-degree polynomial.
This situation is also called **high variance**. This means that your algorithm can't do accurate predictions - by changing the input data only a little, the model output changes very much.

| ![overfitting.jpg](./img/overfitting.jpg) |
|:--:|
| <b>Onderfitting. The 13-degree polynomial model is applied to cubic data. Image by Author</b>|

These are two extremes of the same problem and the optimal solution always lies somewhere in the middle.

| ![good_model.jpg](./img/good_model.jpg) |
|:--:|
| <b>Good model. The 5-degree polynomial model is applied to cubic data. Image by Author</b>|

I will not talk much about bias/variance trade-off, you can read about it [here](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229).

| ![bias_variance_four_plots.jpg](./img/bias_variance_four_plots.jpg) |
|:--:|
| <b>Bias and Variance options on four plots. Image by Author</b>|

But let me briefly mention possible options:
- low bias, low variance - just right.
- low bias, **high variance** - **overfitting** - the algorithm outputs very different predictions for similar data.
- **high bias**, low variance - **underfitting** - the algorithm outputs similar predictions for similar data, but predictions are wrong (algorithm "miss").
- high bias, high variance - very bad algorithm. You will most likely never see this.

All these cases can be placed on the same plot. It is a bit less clear than the previous one but more compact.

| ![bias_variance_one_plot.jpg](./img/bias_variance_one_plot.jpg) |
|:--:|
| <b>Bias and Variance options on one plot. Image by Author</b>|

## Tools

Now let's look at techniques of fighting with underfitting and overfitting, considering exactly *why you should use them*.

### General Idea You Should Remember



### More Features / Less Features



### More Regularization / Less Regularization



### Why Getting More Data Sometimes Can't Help





## Summary

Table


Ceiling analysis
Error analysis

Based on the results of the quality assessment, it is very important to understand exactly where to move and what to pay attention to.

Otherwise, you can spend days and weeks of work on improving the part of the model that will not give a large increase in total quality.