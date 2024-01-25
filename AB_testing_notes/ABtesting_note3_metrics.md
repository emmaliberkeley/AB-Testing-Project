# AB Testing Choosing and Characterizing Metrics
1. How to choose metrics:
    1. Think about what you are going to use the metrics for 
        1. Invariant checking (sanity checking): those are the metrics that should change across experiment and control, eg. Are the population the same across C and E? Are they have the same amount of users? Do you speak comparable amount of languages. 
        2. Evaluation: test the performance of what you care
            1. High level business metrics: top line, market share, # of users
            2. Detail metrics on user experience research: how long users stay on your page. Eg. users do not finish the online classes, so dig into it. Brain storm metrics. Some metrics are difficult to evaluate. If you care about if students get more jobs after taking the online courses, hard to collect info about students' job situation and need to take a really long time to see effect (find a job)
2. Metrics definition: 
    1. One sentence to define the metrics so everyone understand:   
        1. eg. active users: how do you define what active is? 7 days or 28 days. Which events are counted as active? Auto notification is not active
        2. Aggregation: sum, mean, median, etc
3. How many metrics?
    1. For sanity checking, you need multiple metrics
    2. For evaluation, some companies want just one metrics so that different teams are working toward one goal collectively. If you have mutiple metrics, you can create a composite metric: Objective Function or OEC(overall evaluation criterion) which is a weighed metrics of all individual metrics. Not advised because need to define all of the individual metrics and may lead to confusion. 
    3. How applicable the metric is?
        1. Better to use more generalized metrics that you can use across a suite of AB tests than coming up with a customized metric for each test that could bring risks. It is better to use metric that is less optimal for your test, if you can use it across the suites that you can do the comparison, than it is to come up with the perfect metric for your test
4. Chooseing a metric:
    1. Business objective: helping students find jobs and financially sustainable with coaching services. Each funnel is a metric. We choose metric based on the business objective. Sometimes we choose #of users who each that point, or the rate/probability such as click through rate/probability of the "start now" button
    ![Alt text](<screenshots/Screenshot 2023-12-30 at 11.42.05 AM.png>)
5. Difficult metrics    
    1. Don't have access to the data such as user happiness
    2. Takes too long, such as finding a job or taking the second course
6. Types of data
    1. Own existing data
        1. Retrospective observational analysis: how the historical data change to develop theories or get a baseline. Can use it with surveys, user experience research to develop ideas. Only correlation not causation     
        2. Running experiment to valide the causation
        3. Ask colleagues and also corporate culture 
    2. External data: provide a baseline to compare with your own data and do a sanity check
        1. third party vendors
        2. Reseach paper or public data
7. Gathering additional data
    1. UER (User experience research)
        1. +Good for brainstorming
        2. +Can you special equipment such as eye tracking equipment to see what users are looking at on the site
        3. -Need to validate the result with the retrospective studies
    2. Focus group 
        1. +Get feedbacks on hypothesis
        2. -Run the risk of group thinking
    3. Survey
        1. +Useful to gather metrics that you cannot gather directly by yourself
        2. -Cannot directly compare with other results because users' answers can be influenced by how the questions are phrazed 
    4. Eg
        1. You can compare the homepage visit and the completion with the external data. If the completion of one particular course is very low, then we can design a UER to see what metric is correlated to the low completion. Maybe the loading time is too long. You can them validate your metric with retrospective analysis to see how that metric is varied over time or conducting new experiments to see how that metric varies as you make changes. 
        2. If you care about if students can find a job after using your product, this metric is hard to measure, so a surveys are helpful. Eg. any questions mentioned in class is touched in interviews. However, the survey result cannot be used to compare with other results because the population from your internal metrics and from your surveys may not be comparable. The population in the survey may be biased relative to the population taking the class
8. Metric definition and data capture
    1. What data are we going to capture? What's in the numerator and denominator? 
    2. Are we going to filter the data? 
    3. How to summarize the metrics? Sum, median, avg
    4. Understand what changes in your data you system can or cannot product. For example, a 15% change of click through rate is unrealistic 
    5. Can use cookie to track the clicks. Javascrip pin is used to track the clicks. Pin is a small piece of code that sends data to a server when an event occurs. Some old browsers do not have JavaScript at all or other browsers IE vs Safari have different failure rate. Different clicks rate across browsers, so need to talk with engineers about it
    6. If a user visit the site on one day and did nothing. The other day come again and wait for 15 mins to click. Do you consider them as separate record or one? Is 15 mins too long? Is one day too long? 
    7. Plot your data over the course of the day or week or even hour to see weekday/weekend effect, time of the day effect. Usually a big dip at midnight. If someone visit the site at 11:50pm and click at 12:01 am, do we count it as part of the first day or the second day's data.
9. Metric definition example:
    1. High level metric: click-through probability = #users who click/#users who visit
    2. #1 Specified definition: a cookie represents a unique user. for each 'time interval', #cookies that click/#cookies. ![Alt text](<screenshots/Screenshot 2024-01-01 at 12.05.59 PM.png>)
    3. #2 Specified definition: create a unique id for each pageview and when user clicks, record the corresponding parent page view. #pageviews with clicks/#pageviews. Also need time interval. The difference between def 1 and def 2 is that def 2 takes into account the page refreshing. ![Alt text](<screenshots/Screenshot 2024-01-01 at 12.12.42 PM.png>). Def 1 and def 2 are almost indistinguishable if you choose a short time interval. def 2 is eaiser to compute
    4. #3 Specified definition: #clicks/#pageviews, which is CTR
10. Filtering and segmenting    
    1. Filter out the results that are malicious clicks  or abusion on your site from your competitor and also the spikes in traffic due to experiment
    2. Filter to the segment you care about for example you only care about a region then filter to that region so that you do not dilute the result, which increase the power and sensitive of your experiment
    3. Be careful not to introduce bias to your data by doing the filtering. For example, people filter out long sessions of user behaviors, but need to make sure that the long sessions is not due to the website, metric, or logging's issue. 
    4. Slicking you data by region, platform, lauguage is a good way to see if your data is biased or not. See if your traffic is disproportionally from one of those slices or not. Also looking at week over week or day over day data or slicing down to individual user group is a great way to identify spam because they usually from the same IP address or come all at the same time  ![Alt text](<screenshots/Screenshot 2024-01-01 at 2.30.14 PM.png>) The graph shows that the spike is not due to weekly variation since the week-by-week graph which current data point/corresponding data point a week ago. ![Alt text](<screenshots/Screenshot 2024-01-01 at 2.32.26 PM.png>) Then split by population and find in Berzerkistan there is a huge spike. It might be a spam so talk to the engineer
        1. Eg: what to see if the clicks are double counted on mobile
        ![Alt text](<screenshots/Screenshot 2024-01-01 at 2.36.59 PM.png>)
        If have rate and probability on the same graph, do cannot know fore sure that if the rate being higher than probability is a consisten partern or if something is off
11. Summary Metrics:
    1. Consider the sensitivity and robustness. Want your metric to be sensitive enough to detect future events. Mean is too sensitive while median is too robust. So better to use 90 percentile: capture the behavior of the majority of the data while still accounting for potential outliers
    2. Data distribution: use histrogram to see the distribution and decide whether to use mean or median if normally distributed or use 75% 90% pencentile or skewed distributions ![Alt text](<screenshots/Screenshot 2024-01-01 at 3.23.52 PM.png>)
    3. Can check out past experiment or do a restrospective analysis
    4. Example:  choose summary metric for video latency  
        1. Setting: several comparable videos with their latency density line (based on their histogram)
        2.![Alt text](<screenshots/Screenshot 2024-01-01 at 3.56.52 PM.png>) ![Alt text](<screenshots/Screenshot 2024-01-01 at 3.54.18 PM.png>)Since the videos are comparable, so their metric shouldn't move around across videos
        3. Add an experiment: vidoes from 1-5 each has highest to lowest resolution, thus their corresponding latency should be from highest to lowest![Alt text](<screenshots/Screenshot 2024-01-01 at 3.57.23 PM.png>)![Alt text](<screenshots/Screenshot 2024-01-01 at 3.59.06 PM.png>) Thus 85 percentile is the most robust and sensitive metric
12. Absolute or relative difference (%change)
    1. relative difference (%change or ratios) preferred: only need to choose one practical significance boundry 
    2. If don't have a good understanding of the metrics, then start with absolute difference and work your way up
    3. Absolute vs. relative difference: Suppose you run an experiment where you measure the number of visits to your homepage, and you measure 5000 visits in the control and 7000 in the experiment. Then the absolute difference is the result of subtracting one from the other, that is, 2000. The relative difference is the absolute difference divided by the control metric, that is, 40%.
    4. Relative differences in probabilities: For probability metrics, people often use percentage points to refer to absolute differences and percentages to refer to relative differences. For example, if your control click-through-probability were 5%, and your experiment click-through-probability were 7%, the absolute difference would be 2 percentage points, and the relative difference would be 40 percent. However, sometimes people will refer to the absolute difference as a 2 percent change, so if someone gives you a percentage, it's important to clarify whether they mean a relative or absolute difference!
13. Variability
    1. ![Alt text](<screenshots/Screenshot 2024-01-04 at 6.33.42 PM.png>)
    2. Probability: if metris is a probability, then it follows binomial distribtution, which approximate normal distribution with n*p> 10. 
    3. Median/percentile: need to know the underlying data. If the underlying data is not normally distributed, then the median's distribution is not normal either
    4. Count/difference: usually normally distributed. The difference between two group is Var(X) + Var(Y)
    5. Rate: rate follow poisson distribution, within a specific time interval, number of customers come to the restaurant for example. However, in AB testing you need to compare the two rate, then the difference of the rate is not poisson distribution, more complex. 
    6. Ratio: such as the P_exp/P_cont. Hard to tell
    ![Alt text](<screenshots/Screenshot 2024-01-04 at 6.44.09 PM.png>)
    7. SD of a sample VS the SD of sampling distribution of the mean (Standard error)
        1. For the exmaple above, SD of a Sample: This measures how much individual data points in a sample deviate from the sample mean. $s = \sqrt{\frac{\sum_{i=1}^{n} (x_i - \bar{x})^2}{n-1}}$
        2. SD of the sampling distribution of the mean or the standard error. This measures how much the sample mean of a distribution is expected to vary from the true population mean.$SE = \frac{s}{\sqrt{n}}$ s is the standard deviation of the sample, and n is the number of data points in the sample. When we have the sigma, use sigma instead which is the population SD. For a small sample, use SD of sample (s) to approximate the population SD (σ) 
14. Nonparametric tests:
    1. Distribution-Free: Nonparametric tests don't require the data to follow a specific distribution. This is particularly useful when you cannot assume that your data follows a normal distribution
    2. Types of Data: Nonparametric tests are often used for ordinal data (data that can be ranked but not measured numerically, like satisfaction ratings) or nominal data (categorical data without a natural order, like colors or brands)
    3. Less Powerful: If the assumptions of a parametric test are met, parametric tests are usually more powerful (i.e., more likely to detect a true effect when there is one) than nonparametric tests. However, when those assumptions are not met, nonparametric tests provide a viable alternative.
15. Empirical Variability
    1. A versus A experiment: The purpose of an A/A test is not to test the effectiveness of two different versions, but rather to check the reliability and accuracy of the testing infrastructure itself.
    Validation of Testing Systems: An A/A test can help ensure that the system used for split testing (like a website's A/B testing tool) is working correctly. If the system is functioning properly, the results from both groups in an A/A test should be statistically identical since both groups are experiencing the same conditions.
    2. Detection of Bias or Errors: If the results from the two groups in an A/A test are significantly different, it might indicate a problem with the testing setup, such as improper randomization
    3. Baseline Metrics: A/A tests can establish baseline metrics for things like conversion rates, click-through rates, or other user behaviors. This baseline can be used for future A/B tests  to compare against and to calculate the sensitivity and power of the tests. If see a lot of variability in a metric in a A versus A test, it's too sensitive to be useful
    4. False Positive Rate: A/A tests are also used to determine the false positive rate of the experimental setup. By conducting an A/A test, where there is knowingly no real difference between the two groups, you can measure how often your testing procedure finds a "significant" difference purely by chance. This rate of finding a significant difference when none should exist is the false positive rate.
16. Empirical Variability: Sanity Checking
    1. https://docs.google.com/spreadsheets/d/17wWNY2jkDlG9BDMYQq2l-ku_8HGajXuF2Zvy__dBEL4/edit#gid=0
    2. Compare result to what you expect: How many experiments will show a statistical significant difference between the two groups at 95% level. Even though there is no difference between the two groups, the true value, in this case 0, will only be captured 95% of the time. Thus, 1 out of 20 experiment is expected to have significant difference on average. ![Alt text](<screenshots/Screenshot 2024-01-04 at 8.14.12 PM.png>)
17. Empirical Confidence Intervals
    1. Estimate variance and calculate confidence interval: keep using the example above. Assume the metric follows a normal distribution, which you can see through histogram. ![Alt text](<screenshots/Screenshot 2024-01-04 at 8.31.34 PM.png>). Empirically we calcualted one margin of error across all the experiment. If we did it analytically, we will get a slightly difference margin of error due to difference SE for each experiment. SE_pool vary but always pretty close to the SD. 
    2. If you metric does not follow normal distribution, direclty estimate CI through AA test. Then get the lower and upper bound of the AA test result and disgard the lower and update 2.5%, which gives 95% CI. 
18. Empirical Variability: Bootstrapping
    1. random sample from each side use as a simulated experiment. Repeat to get many experiments.
    2. Empirical estimate, best to have 40+ experiments
19. Lessons Learned
    1. Metric definition: Spend most of the time defining metric: for exmaple, CTR, using page view or impression. The first page that user visits or all the pages that a user visit. US only or globally. Removing spam or not removing spam. Another example is latency. How long does it take the page to load? Are you talking about when the first byte loads or the last byte loads. Need to spend time agree on what are going to be measured. 
    2. Sensitivity and robustness: latency usually has a lumpy distribution, mostly due to users with difference connection speed. Try not to use mean because otherwise even if there are improvement, the mean or any central measure do not move. Look for the higher percentile metric to move when we did some positive impact on the latency. Another example is tasks per user per day in search. It is a very stable metric since people only search a fixed number of times per day. Makes for sense to measure per week or per two weeks rather than by daily. If you metric has a big weekly variability, then per 28 days makes more sense than 30 days. 






        

