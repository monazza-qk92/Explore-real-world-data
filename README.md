# Exploring data with Python - real world data


Last time, we looked at grades for our student data, and investigated this visually with histograms and box plots. Now we will look into more complex cases, describe the data more fully, and discuss how to make basic comparisons between data.

## Real world data distributions
Last time, we looked at grades for our student data, and estimated from this sample what the full population of grades might look like. Just to refresh, lets take a look at this data again.

![image](https://user-images.githubusercontent.com/79583184/200015910-838f954f-5be2-426b-a579-c26cad4da362.png)

The distribution of the study time data is significantly different from that of the grades.

Note that the whiskers of the box plot only begin at around 6.0, indicating that the vast majority of the first quarter of the data is above this value. The minimum is marked with an o, indicating that it is statistically an outlier - a value that lies significantly outside the range of the rest of the distribution.

Outliers can occur for many reasons. Maybe a student meant to record "10" hours of study time, but entered "1" and missed the "0". Or maybe the student was abnormally lazy when it comes to studying! Either way, it's a statistical anomaly that doesn't represent a typical student. Let's see what the distribution looks like without it.

![image](https://user-images.githubusercontent.com/79583184/200016079-e53de149-73b9-4c3b-a2ce-317dc226c35f.png)

For learning purposes we have just treated the value 1 is a true outlier here and excluded it. In the real world, though, it would be unusual to exclude data at the extremes without more justification when our sample size is so small. This is because the smaller our sample size, the more likely it is that our sampling is a bad representation of the whole population (here, the population means grades for all students, not just our 22). For example, if we sampled study time for another 1000 students, we might find that it's actually quite common to not study much!

When we have more data available, our sample becomes more reliable. This makes it easier to consider outliers as being values that fall below or above percentiles within which most of the data lie. For example, the following code uses the Pandas quantile function to exclude observations below the 0.01th percentile (the value above which 99% of the data reside).

![image](https://user-images.githubusercontent.com/79583184/200017362-fb8f7f52-7c2c-4153-ad08-2d8f2e6c48f7.png)

With the outliers removed, the box plot shows all data within the four quartiles. Note that the distribution is not symmetric like it is for the grade data though - there are some students with very high study times of around 16 hours, but the bulk of the data is between 7 and 13 hours; The few extremely high values pull the mean towards the higher end of the scale.

Let's look at the density for this distribution.

![image](https://user-images.githubusercontent.com/79583184/200017449-1b40179b-e978-4d0d-9e1b-88aadc3bca07.png)

This kind of distribution is called right skewed. The mass of the data is on the left side of the distribution, creating a long tail to the right because of the values at the extreme high end; which pull the mean to the right.

Measures of variance
So now we have a good idea where the middle of the grade and study hours data distributions are. However, there's another aspect of the distributions we should examine: how much variability is there in the data?

Typical statistics that measure variability in the data include:

Range: The difference between the maximum and minimum. There's no built-in function for this, but it's easy to calculate using the min and max functions.
Variance: The average of the squared difference from the mean. You can use the built-in var function to find this.
Standard Deviation: The square root of the variance. You can use the built-in std function to find this.

### Grade:
 - Range: 94.00
 - Variance: 472.54
 - Std.Dev: 21.74

### StudyHours:
 - Range: 15.00
 - Variance: 12.16
 - Std.Dev: 3.49
 
 Of these statistics, the standard deviation is generally the most useful. It provides a measure of variance in the data on the same scale as the data itself (so grade points for the Grade distribution and hours for the StudyHours distribution). The higher the standard deviation, the more variance there is when comparing values in the distribution to the distribution mean - in other words, the data is more spread out.

When working with a normal distribution, the standard deviation works with the particular characteristics of a normal distribution to provide even greater insight. Run the cell below to see the relationship between standard deviations and the data in the normal distribution.

![image](https://user-images.githubusercontent.com/79583184/200017786-05454cf3-7f42-48a8-b611-b58a10a6553d.png)

The horizontal lines show the percentage of data within 1, 2, and 3 standard deviations of the mean (plus or minus).

In any normal distribution:

Approximately 68.26% of values fall within one standard deviation from the mean.
Approximately 95.45% of values fall within two standard deviations from the mean.
Approximately 99.73% of values fall within three standard deviations from the mean.
So, since we know that the mean grade is 49.18, the standard deviation is 21.74, and distribution of grades is approximately normal; we can calculate that 68.26% of students should achieve a grade between 27.44 and 70.92.

The descriptive statistics we've used to understand the distribution of the student data variables are the basis of statistical analysis; and because they're such an important part of exploring your data, there's a built-in Describe method of the DataFrame object that returns the main descriptive statistics for all numeric columns.

![image](https://user-images.githubusercontent.com/79583184/200018000-a2f85a2a-4e15-482e-8485-b96ec83e7639.png)

### Comparing data
Now that you know something about the statistical distribution of the data in your dataset, you're ready to examine your data to identify any apparent relationships between variables.

First of all, let's get rid of any rows that contain outliers so that we have a sample that is representative of a typical class of students. We identified that the StudyHours column contains some outliers with extremely low values, so we'll remove those rows.

![image](https://user-images.githubusercontent.com/79583184/200018179-3581d339-536d-44e4-b6e5-6964095d9366.png)

### Comparing numeric and categorical variables
The data includes two numeric variables (StudyHours and Grade) and two categorical variables (Name and Pass). Let's start by comparing the numeric StudyHours column to the categorical Pass column to see if there's an apparent relationship between the number of hours studied and a passing grade.

To make this comparison, let's create box plots showing the distribution of StudyHours for each possible Pass value (true and false).

![image](https://user-images.githubusercontent.com/79583184/200018624-8ab78a1b-6482-4533-84a2-d40150597676.png)

Now let's compare two numeric variables. We'll start by creating a bar chart that shows both grade and study hours.

![image](https://user-images.githubusercontent.com/79583184/200019367-1808832c-02d8-4bf8-ad5c-064ea4092e0d.png)

The chart shows bars for both grade and study hours for each student; but it's not easy to compare because the values are on different scales. Grades are measured in grade points, and range from 3 to 97; while study time is measured in hours and ranges from 1 to 16.

A common technique when dealing with numeric data in different scales is to normalize the data so that the values retain their proportional distribution, but are measured on the same scale. To accomplish this, we'll use a technique called MinMax scaling that distributes the values proportionally on a scale of 0 to 1. You could write the code to apply this transformation; but the Scikit-Learn library provides a scaler to do it for you.

![image](https://user-images.githubusercontent.com/79583184/200019470-7ac5c795-5231-4a94-ac77-ca99fbb12f92.png)


With the data normalized, it's easier to see an apparent relationship between grade and study time. It's not an exact match, but it definitely seems like students with higher grades tend to have studied more.

So there seems to be a correlation between study time and grade; and in fact, there's a statistical correlation measurement we can use to quantify the relationship between these columns.Another way to visualise the apparent correlation between two numeric columns is to use a scatter plot.

![image](https://user-images.githubusercontent.com/79583184/200019742-8e4c6c9f-dea1-439c-91e8-adfa3ba79bbe.png)

Again, it looks like there's a discernible pattern in which the students who studied the most hours are also the students who got the highest grades.

We can see this more clearly by adding a regression line (or a line of best fit) to the plot that shows the general trend in the data. To do this, we'll use a statistical technique called least squares regression.

![image](https://user-images.githubusercontent.com/79583184/200019871-daedc7a1-ce52-4cb6-9af8-663a963d641d.png)

Fortunately, you don't need to code the regression calculation yourself - the SciPy package includes a stats class that provides a linregress method to do the hard work for you. This returns (among other things) the coefficients you need for the slope equation - slope (m) and intercept (b) based on a given pair of variable samples you want to compare.

Note that this time, the code plotted two distinct things - the scatter plot of the sample study hours and grades is plotted as before, and then a line of best fit based on the least squares regression coefficients is plotted.

The slope and intercept coefficients calculated for the regression line are shown above the plot.

The line is based on the f(x) values calculated for each StudyHours value. Run the following cell to see a table that includes the following values:

The StudyHours for each student.
The Grade achieved by each student.
The f(x) value calculated using the regression line coefficients.
The error between the calculated f(x) value and the actual Grade value.
Some of the errors, particularly at the extreme ends, and quite large (up to over 17.5 grade points); but in general, the line is pretty close to the actual grades.

### Using the regression coefficients for prediction
Now that you have the regression coefficients for the study time and grade relationship, you can use them in a function to estimate the expected grade for a given amount of study.
##### Studying for 14 hours per week may result in a grade of 70

So by applying statistics to sample data, you've determined a relationship between study time and grade; and encapsulated that relationship in a general function that can be used to predict a grade for a given amount of study time.

This technique is in fact the basic premise of machine learning. You can take a set of sample data that includes one or more features (in this case, the number of hours studied) and a known label value (in this case, the grade achieved) and use the sample data to derive a function that calculates predicted label values for any given set of features.

## Summary
Here we've looked at:

1.What an outlier is and how to remove them
2.How data can be skewed
3.How to look at the spread of data
4.Basic ways to compare variables, such as grades and study time










