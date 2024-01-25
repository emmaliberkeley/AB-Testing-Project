# AB Testing Overview
1. AB testing: Allows you to determine scientifically how to optimize a webstie or a mobile app by trying out possible changes and see what performs better with users. Allows using data to make decisions instead of highest-paid person's opinion.
    1. Use AB testing only if we have baseline to compare with. AB testing can help you climb to the top of your current mountain but cannot help you choose whether you choose climb the current mountain or a new one. Need clear control and metrics
        1. Cannot test new new experience: For completely new experiences, there may be no existing baseline or control to compare against. Novelty effect: Users might need time to adapt to entirely new experiences, and their initial reactions might not reflect long-term usage patterns. A/B tests are typically run over a short period, which may not be sufficient to capture the full impact of a new experience.
        2. AB testing cannot tell you if you are missing something
        3. It requires too long to see effect, e.g. purchasing a car, updating brand with new logo
    2. There will be a control group with existing feature and a treatment group with new version. The goal is to determine which one of the feature is better. The effect should be significant, robust and consistent (repeatable result)
        1. Significant: the likelihood that the observed results in a study or experiment are not due to random chance.     
            1. P-value: The significance of the result is often quantified using a p-value. The p-value measures the probability of observing a result as extreme as, or more extreme than, the one observed, assuming that the null hypothesis (no difference between groups) is true. If this p-value is below a predetermined threshold (commonly 0.05), the result is declared statistically significant.
        2. Sensitive: sensitivity can also refer to how responsive a model is to changes in the input variables. A model is said to be sensitive if small changes in the input produce significant changes in the output
        3. Robust: remains effective under a variety of conditions, especially when the assumptions under which the method was developed are not fully met, eg: data do not perfectly meet assumptions like normality or homoscedasticity.
        4. Sensitivity and robustness tradeoff: the ideal situation is that your model or metric pick up changes that you care about but not robust against changes that you don't care about (outliers).
        In many practical situations, increasing a model's sensitivity makes it less robust and vice versa. For instance, a highly sensitive model might pick up true signals effectively but also become more prone to being influenced by outliers or noise in the data. Conversely, making a model more robust, such as by using techniques that downplay or ignore outliers, might lead to it missing subtle but important signals in the data (reduced sensitivity). In some fields, like medical diagnostics, high sensitivity (correctly identifying true cases) is crucial, even if it means compromising robustness slightly. In other areas, such as financial modeling or engineering, robustness might be more important to ensure the model performs consistently across various unpredictable real-world conditions.
        5. Sensitivity in Statistical analysis vs machine learning
            1. Sensitivity in machine learning: recall, measure the model's ability to correctly identify true positives
            2. Sensitivity in Statistical analysis: the degree to which the model's output or predictions are  influenced by fluctuations or variations in its input data.
2. Types of AB testing: 
        1. change of UI color for example can be an obvious AB testing; 2
        2. Ranking change is a more subtle AB testing that is hard to tell by the users. It changes the order or the position of the items (search result, product listings, content feeds)and see which how it influence users' behavior through metrics like click through rate, time spent on the site, or purchase rate of the product; 
        3. page load time: for every 100 milisecond they added to the page load time, they had a  1% decrease in revenue(Amazon) on average. 
3. Overview: choose a metric, review statistics, design, analyze
4. Other types of testing other than AB testing:
    1. We can use the log of user activities to see if there is any interesting patterns, hence generate a hypothesis about what causes the changes in the user behavior. Then you can randomize and design an experiment to do a perspective analysis. You can use test out whether you can make the hypothesis happen with an experiment, then you can use AB testing to compare the result and see if the result if the theory is valid. "using user activity logs to identify patterns, generating hypotheses, designing an experiment, and using A/B testing to validate the results."
    2. User experience research, focus groups and survey, and human evaluation. Those techniques give qualitative data while AB testing gives quantitative data, which is complimentary to AB testing
5. Example: Changing the homepage "start now" button from orange to pink will increase how many students explore the site
    1. customer funnel from top to bottom: ![Alt text](<screenshots/Screenshot 2023-12-28 at 9.38.42 PM.png>)
    2. Metrics: 
        1. Total number of courses completed: Takes too much time
        2. Number of clicks on the "start now": increase the rate user progress down the funnel at one level will have a positive impact of the end of the funnel as well. This is not good because what if more users view the homepage and hence more clicks? We want to know the percentage of clicks/views
        3. CTR (click through rate): Number of clicks on the "start now"/total number of page views
        4. CTP (click through probability): number of unique visitors who click at least once/number of unique visitors who view the page.  Going to use this one because we are interested in how often users progress to the second level of the funnel. Need to match each page view with all the child clicks so that you count at most one child click per page view 
        5. When to use rate or probability: use rate when you want to measure the userbility of the site (how often do users choose that button to click on out of all of the different buttons) and use probability when you want to measure the total impact or progression (how often users go to the second page on your site)
    3. updated hypothesis: Changing the homepage "start now" button from orange to pink will increase click through probability of the button
    4. Binomial distribution with (sampling distribution context, with large sample size>30, apply CLT): miu = p, standard deviation of the sampling distribution or standard error = sqrt(p*q/n): 
        1. 2 types of outcomes: s and f
        2. Independent events
        3. Identitically distributed 
        4. Eg: 
            1. yes binomial: rolling a die 50 times, outcome 6 or other; student completion of the course after 2 month, outcome complete or not complete (usually each student use only one account, so we can assume each student is independent from each other)
            2. No not binomial: clicks on a search engine results page, outcome click or not click. One user might search one key word but find it not good and try some other key words that are highly related to the previous one, but you cannot tell if they are from the same user; similary purchase of items within a week, outcome purchased or not purchased. One user could add multiple items to the shopping cart, then each item is related to other items and we cannot tell if they are from the same user
            3. Click through probability follow binomial distribution
                1. Indepedent: whether one unique visitor clicks or not has no influence on whether another unique visitor clicks
                2. Identically distributed: "Identically distributed" means that each event (in this case, each unique visitor's decision to click or not click) comes from the same probability distribution with the same underlying probability structure. This assumes that the population of unique visitors has a consistent behavior pattern regarding the probability of clicking.
                3. Click-Through Probability for Individual Users: When considering whether a unique user chooses to click or not, the scenario fits the characteristics of a binomial distribution. In this case, each user has two possible outcomes (click or no click), akin to a "success" or "failure" in binomial terms. If you have a large number of users, each with a certain probability of clicking, and their actions are independent of each other, then the number of users who click follows a binomial distribution.
                4. Overall Click-Through Probability for a Web Page: When talking about the click-through probability for a web page or an advertisement, you're essentially aggregating these individual binary outcomes (clicks and no clicks) across many users. The overall click-through probability is essentially the mean success rate (clicks) in a series of independent, identically distributed binary trials (each user’s decision to click or not). This aggregate measure is a property of the binomial distribution, as the binomial distribution describes the number of successes in a fixed number of independent binary trials.
        5. Confidence interval 95%: if we repeat the experiment over and over again, confidence interval around the sample mean will cover the true value in the population 95% of the time. 
            1. margin of error (m): z_score of the confidence level * SE
                1. SE = SD/sqrt(n)
                2. What is SE: The standard error measures how much the sample mean is expected to vary from one sample to another. It provides an estimate of the variability of sample means around the true population mean. The test statistic determines how far the sample statistic (like the mean) deviates from a null hypothesis value, normalized by the standard error.Imagine you conduct a study to estimate the average height of adult men in a city. From your sample, you calculate the average height as 175 cm with a standard deviation of 5 cm. If your sample size is 100, the standard error of the mean would be 5/sqrt(100)=0.5cm. This standard error tells you that if you were to repeat the study with new samples of 100 men each time, the average height you calculate each time would typically vary by about 0.5 cm from the true average height of all adult men in the city.
                3. Confidence interval: x_bar +_ m
            2. Use normal to approximate binomial if n*p > 5 and n*q > 5. 
            3. ![Alt text](<screenshots/Screenshot 2023-12-29 at 11.47.58 AM.png>)
            4. If you mean is 0.1, then with N = 1000, you will see clicks from 81 to 119, outside this range, result should be surprising
        6. Binomal with count of success and sample distribution: ![Alt text](<screenshots/Screenshot 2023-12-29 at 11.29.51 AM.png>)
        7. Central Limit Theorem: https://www.scribbr.com/statistics/central-limit-theorem/
            1. The central limit theorem relies on the concept of a sampling distribution, which is the probability distribution of a statistic for a large number of samples taken from a population. CTL: the sum (or average) of a large number of independent and identically distributed random variables will be approximately normally distributed, regardless of the underlying distribution
            2. Imagining an experiment may help you to understand sampling distributions: Suppose that you draw a random sample from a population and calculate a statistic for the sample, such as the mean. Now you draw another random sample of the same size, and again calculate the mean. You repeat this process many times, and end up with a large number of means, one for each sample. The distribution of the sample means is an example of a sampling distribution. The central limit theorem says that the sampling distribution of the mean will always be normally distributed, as long as the sample size is large enough. Regardless of whether the population has a normal, Poisson, binomial, or any other distribution, the sampling distribution of the mean will be normal.
            ![Alt text](<screenshots/Screenshot 2023-12-29 at 11.39.15 AM.png>)
            the term "sample size" refers to the number of individual data points in a single sample.
6. Hypothesis testing: calculate P(results due to chance if the null if true)
    1. Null hypothesis: H_0 P_control = P_experiment 
    2. Alternative hypothesis: H_A P_control != P_experiment 
    3. Meaure P(P_exp-P_cont|H_0) 
    4. Reject null if the probability in step 3 is small enough based on the cutoff or alpha or level of significance
    5. Two tails vs one tail: If you're going to launch the experiment for a statistically significant positive change, and otherwise not, then you don't need to distinguish between a negative result and no result, so a one-tailed test is good enough. If you want to learn the direction of the difference, then a two-tailed test is necessary. In one tail test, the t_score or z_score cutoff is 1.645. The null is that the experiment group is less effective than the control or have the same effect. The alternative is that the experiment group is more effective than the control.
7. Pooled standard error 
    1. Control side may have different number of users from the experiment side. Need to compare proportion of clicks from both side. See if the difference of the proportion is due to chance or extremely unlikely if the null is true
    2. Since we have two samples, we need a pooled standard error that help compare with both ![Alt text](<screenshots/Screenshot 2023-12-29 at 4.08.15 PM.png>)
    (d_hat - miu)/SE_pooled ~ N(0, 1)
8. Practically significant or subtantive in addition to be statistically significant 
    1. Cannot implement every change that's statistically significant because of the cost of implementation or may want to wait for more substantial change 
    2. What's considered pratically significant varies among industries: in google online ad, 1% change in click through rate is huge but in medical industry maybe need 10-15% change, which is the bar for being practically significant 
    3. You want your experimental result to be repeatable and robust and also practically significant 
    4. Effect size is the magnitude of a difference between groups. It indicates the practical significance of a finding.
9. Power:
    1. Statistical power (1-beta) usually at 80%, or sensitivity, is the likelihood of a significance test detecting the true effect, avoiding a Type II error (False Negative). The higher the statistical power of a test, the lower the risk of making a Type II error. The significance level (alpha) of a study is the Type I error probability, and it’s usually set at 5%. Confidence level is 1-alpha. While high-powered studies can help you detect medium and large effects in studies, low-powered studies may only catch large ones.
    2. How to increase power:   
        1. Increase the effect size.
        2. Increase sample size. But there is a point at which increasing your sample size may not yield high enough benefits. Increasing the sample size is a common way to increase power without affecting α.
        3. Increase the significance level. When you increase the significance level, you are making it easier for a test to declare a result as statistically significant. This means that the test is less stringent in requiring strong evidence to reject the null hypothesis. As a result, the test is more likely to detect small effects or differences, which increases its statistical power. When you decrease the significance level, your significance test becomes more conservative and less sensitive to detecting true effects.
        4. Reduce measurement error.
    3. Type I (alpha) error and Type II (Beta) error trade off: Increasing one will decrease the other. Type I error is false positive, or incorreclty rejecting a true null. Type II error is false negative, or fail to reject a false null. 
    1-alpha: correctly rejecting a fase null (avoiding type 1 error);
    1-beta: correctly detecting the true effect (avoiding type 2 error)
    https://www.scribbr.com/statistics/type-i-and-type-ii-errors/
    https://www.scribbr.com/statistics/statistical-power/
    4. Calculate the sample size
        1. Baseline conversion rate (the conversion in the control)
        2. Minimum detectable effect: or minimum difference (d_min) or practical significance level 
        3. Alpha: usually at 5%
        4. Power: 1-beta usually at 80%
        5. Factors influencing the sample size: 
            1. significance level
            2. MDE: minimum detectable effect
            3. power (1-beta)
            4. standard error![Alt text](<screenshots/Screenshot 2023-12-29 at 10.02.10 PM.png>) if the p is below 1/2, the p*(1-p) equation peak at p = 1/2 since it also equals to -(p-1/2)^2 +1/4, before p = 1/2, as p increase, the standard error increases as well, which increases the sample size based on the equation from wiki ${\displaystyle n\geq \left({\frac {z_{\alpha }+\Phi ^{-1}(1-\beta )}{\mu ^{*}/\sigma }}\right)^{2}}$
    5. Analyze the result![Alt text](<screenshots/Screenshot 2023-12-29 at 10.30.08 PM.png>)
        1. The null is that d = 0. Since d = 0 is not in the confidence interval, we can reject the null with 95% confidence level. Based on the confidence interval, we will have at least 2% change with the launch of new version. Our practical significance d_min = 2%, that the result also pass the practical significance level. We can launch the new version 
        https://en.wikipedia.org/wiki/Sample_size_determination
        2. ![Alt text](<screenshots/Screenshot 2023-12-29 at 10.43.09 PM.png>)
            1. Eg1: launch which is the situation in the example above
            2. Eg2: this confidence interval is neutral, not launch
            3. Eg3: it is statistically significant but not practically significant, meaning the level of increase in the click through probability too small to care about, not launch
            4. Eg4: the variability is too large. Do not have enough power to draw a strong conclusion. Need additional test
            5. Eg5: CL overlaps 0, so there might not be a significant change even though some part of CL pass the practical significance. Need additional test
            6. Eg6: It's possible to not pass the practical significance level. Need additional test

