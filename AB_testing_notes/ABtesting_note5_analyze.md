# Analyzing Result
1. Sanity checks: 
    1. Experiment Setup
        1. Objective: To determine if the new feature increases the number of button clicks.
        2. Unit of Diversion: Event ID, where each event is a user clicking the button.
        3. Groups: Users are randomly assigned to either the control group (without the new feature) or the treatment group (with the new feature).
    2. Sanity Check Steps
        1. Check Randomization: Verify equal distribution: Let's say you expect 50,000 events in total. You check and find 25,000 events in the control group and 25,000 in the treatment group, confirming an equal distribution.
        2. Validate Event Tracking: Ensure consistent tracking: Confirm that clicks are being recorded similarly in both groups.
        3. Data integrity: Double-check that there are no missing event IDs and that the data logged for each click is complete.
        4. Confirm Event Definition: Consistent definition: Ensure that a "click" is defined the same way in both groups.
        5. Analyze Baseline Metrics: 
            1. Pre-experiment metrics: Review the average number of daily clicks before the experiment started. 
            2. Compare with historical data: Confirm that the click rates before the experiment were consistent with historical trends.
        6. Assess Traffic and User Distribution:
            1. Traffic consistency: Check that the proportion of users in each group reflects the overall user base demographics.
            2. User behavior patterns: Look for any abnormal patterns in user behavior that could indicate an issue with the setup.
        7. Look for Anomalies
            1. Identify outliers: Check for any unusual spikes or drops in the number of clicks that might indicate tracking issues.
            2. Data quality issues: Look for duplicate event IDs or other signs of data corruption.
        8. Perform a Basic Statistical Test: 
            1. Statistical equivalence: Use a chi-square test to confirm there's no statistically significant difference in the distribution of events between the groups at the start of the experiment.
    3. Example Outcome
        1. Randomization Check: Found an equal number of events in both groups.
        2. Event Tracking: No missing or duplicate event IDs, and click tracking is consistent.
        3. Event Definition: "Click" is uniformly defined across both groups.
        4. Baseline Metrics: Click rates before the experiment align with historical data.
        5. Traffic and User Distribution: User distribution is consistent with the overall user demographics.
        6. Anomaly Check: No unusual data patterns found.
        Statistical Test: Chi-square test confirms no significant difference in click distribution between groups at the experiment's start.
2. Choosing Invariants
![Alt text](<screenshots/Screenshot 2024-01-08 at 8.53.48 PM.png>)   
![Alt text](<screenshots/Screenshot 2024-01-09 at 9.25.48 PM.png>)
Since both the control group (original sign-in button location) and the experimental group (new sign-in button location) will be using the same underlying infrastructure for video loading, any changes in the video load time would likely be unrelated to the position of the sign-in button.
3. Checking invariant
![Alt text](<screenshots/Screenshot 2024-01-09 at 9.36.15 PM.png>)
This is a little different then thee approach in lesson one, where we computed the confidence interval around the observed click-through probability. In that case, we didn't know the true click-through probability. But here, I know that if the experiment is set up properly, the true probability is 0.5. So, I can compute that confidence interval. In most empirical experiment, the p (true population proportion) is unknown, so we do not typically use it. Usually p_hat is the best estimate of the population proportion based on the available sample.to caculate SE, confidence interval all use p_hat in most cases. 
![Alt text](<screenshots/Screenshot 2024-01-09 at 9.46.48 PM.png>)    
11/14 days more cookies in the control than the coolies in the experiment. Need to talk to engineers. Can try slicing by country for example to see if one particular slice is causing the problem.     
4. Single Metric: Introduction
    1. Key assumption of significance calculation: that the sample size was fixed in advance. Article about why we should decide on the sample size ahead of time instead of peaking at the result and not finishing the experiment until seeing a significant result. This will lead to much higher false positive rate: https://www.evanmiller.org/how-not-to-run-an-ab-test.html This is because each time you look at the data and conduct a significance test, you're essentially performing multiple comparisons. The false positive rate of 5% applies to each individual test in isolation. If you conduct multiple tests, the probability of at least one false positive increases.
5. Single Metric: Example
    1. Click through rate distribution is more likely to be poission which is harder to deal with than binomial and click through probability is more likely to be binomial. For CTR, good to estimate the variance empirically 
    2. EFFECT SIZE: ![Alt text](<screenshots/Screenshot 2024-01-10 at 2.13.32 PM.png>) A typo: the confidence interval is 0.0220 at lower bound not 0.0020
    3. Sign Test: ![Alt text](<screenshots/Screenshot 2024-01-10 at 3.21.01 PM.png>)![Alt text](<screenshots/Screenshot 2024-01-10 at 3.21.35 PM.png>)
    4. SE for the comparing two groups with different sample size is $SE = \sqrt{\frac{SD_1^2}{n_1} + \frac{SD_2^2}{n_2}}$. If we assume the two sample has the same SD, then the SE of the two sample is $SE = SD \times \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}$
    5. Example: https://docs.google.com/spreadsheets/d/1F4Ld-6gFMPGMijiW52HukJSqF1KN8cyUwIueeGU6PM0/edit#gid=0
6. Simpson’s Paradox
    ![Alt text](<screenshots/Screenshot 2024-01-10 at 3.28.01 PM.png>)
    All women chose to apply hard to get in department, which collectively seems like the admission is biased against women which in fast is biased in favor of women if looking by department
    ![Alt text](<screenshots/Screenshot 2024-01-10 at 3.59.17 PM.png>)
    Thus, need to ensure similar distribution of pageview in control and treatment across difference slices as a sanity check. Even though new users tends to have higher CTR than experienced users, more new users are assigned to control group and more experienced users are assigned to treatment group, making the final CRT in the treatment group less than the CRT in the control group. 
7. Multiple Metrics: Introduction
    1. Multiple comparisons problem: The more you test, the more likely you are to see significant differences just by chance. If you have 20 metrics with 95% CL, you will expect to see one case at least where you got a result that says it's significant when it is not. However, this is not repeatable. If you do the same experiment another day, bootstrapping, or divide into slices, you should not see the same metric showing up as significant differences every time. It should occur randomly. 
        1. To address this problem, statistical methods like the Bonferroni correction are often used. The Bonferroni correction adjusts the significance level downwards to account for the number of tests being conducted. This reduces the overall chance of false positives but also makes it harder to detect true effects (increases the risk of false negatives). 
        2. Ways to address the significance level inflation: 
            1. Bonferroni correction: new significance level = significance level / number of tests. For example if your alpha = 5% and there are 3 tests, the new alpha is 5%/3.Bonferroni correction has been criticized for being too conservative, especially with a large number of tests where metrics are positively correlated and all tend to move at the same time. 
            2. alpha_overall = 1-(1-alpha_individual)**N. You set the alpha_overall to be 5% for example, then get the alpha_individual. Assuming independence of each metric 
            ![Alt text](<screenshots/Screenshot 2024-01-11 at 2.06.44 PM.png>)
            3. False Discovery Rate (FDR): This conservatism increases the chance of false negatives (hard to detect true effect). In response, other methods like the False Discovery Rate (FDR) have been developed to provide a balance between identifying true effects and controlling for false positives.The FDR is the expected proportion of incorrect rejections of the null hypothesis (false discoveries) among all the rejections. A common method for controlling the FDR is the Benjamini-Hochberg procedure. This method involves ranking the individual test p-values, then determining a threshold below which a result is declared significant. This threshold is set to ensure that the expected proportion of false discoveries does not exceed a pre-specified level (e.g., 5%).
                1. Benjamini-Hochberg procedure with an example:
                    1. Rank the p-values: Sort your p-values in ascending order.
                    2. Calculate the critical value for each test: For each p-value, calculate its critical value using the formula (i/N)*α. i is the rank of the p-value (1 for the smallest, 2 for the second smallest, etc.). N is the total number of tests (in your case, 100), and  α is the significance level you've chosen (e.g., 0.05).
                    3. Find the largest p-value that is smaller than its critical value: Starting from the largest ranked p-value, find the p-value that is less than or equal to its corresponding critical value. This p-value and all those smaller than it are considered significant under the BH procedure.
            4. Familywise error rate(FWER): It refers to the probability of making one or more Type I errors (false positives) when performing multiple statistical tests.
            ![Alt text](<screenshots/Screenshot 2024-01-11 at 2.19.15 PM.png>)
            When you have multiple tests in an experiement say 200, using FDR, you will have 5% FDR since 95% of metrics that you are claiming to have significant results actually do. However, if you use FWER, that rate is 1 since in the experiement you have at least 1 metric that is false positive. 
    2. Automatic detection of difference: In automatic detection of differences, algorithms may test a large number of hypotheses simultaneously. For instance, in a complex dataset, an algorithm might examine various combinations of features to find those that significantly affect an outcome. Need to use Bonferroni correction or false recovery rate. 
    3. ![Alt text](<screenshots/Screenshot 2024-01-10 at 9.39.13 PM.png>)
        1. We are assuming that three tests are independent but they actually positively correlated, thus the chance of at least 1 false positive is less than 0.143 but for the ease of calculation, usually assume the tests are independent.  
8. Analyzing Multiple Metrics: 
    1. Multiple Metrics and Their Interactions: When dealing with multiple metrics, it can be tricky as they may not always align. Ideally, related metrics should move in the same direction. For instance, click-through rate and click-through probability are expected to trend similarly.
    2. Composite Metrics: Sometimes, a single metric is composed of multiple underlying factors. An example given is Revenue Per Thousand Queries (RPM), which includes both click-through rate and cost per click. Understanding the movement of composite metrics requires analyzing the individual components.
    3. Conflicting Metrics: In cases where metrics conflict, understanding user behavior becomes complex. For example, a UI change might increase clicks but decrease reading time on a page. Determining the overall impact requires a qualitative evaluation of how users are reacting to the change, as it's not straightforward to quantify which outcome is better.
    4. Overall Evaluation Criteria (OEC): The discussion then shifts to the concept of an OEC, which is a single, overarching metric or criterion that encapsulates what's important for a company. The challenge is in finding a good OEC that balances different factors, such as stay time and clicks. This balance could be in any proportion, like 70/30 or 25/75, depending on what's more relevant to the company's goals.
    5. Developing an OEC: The process of developing an OEC involves business analysis and validation through multiple experiments. It's important to ensure that the OEC aligns with the company's overall objectives, like balancing revenue generation with increased site usage.
    6. Practical Example from Google: The transcript provides an example from Google, where decision-makers were asked to re-evaluate hundreds of experiments without knowing the specific tests or previous decisions. This exercise was used to derive weights for an OEC. However, it was found that even with an OEC, individual metrics were still crucial for decision-making. The OEC served more as a guideline encapsulating the company's values rather than as a strict numerical decision tool.
9. Drawing conclusions: 
    1. Understanding the Impact: Assess if statistically significant results truly affect user experience and comprehend the nature of the change.
    2. Handling Mixed Results: When different metrics yield varying results, analyze why this is happening, drawing on experience and context from previous experiments.
    3. Evaluating Effects on User Segments: Investigate why a change affects different user groups in varying ways. An example given is how bolding text is effective in some languages but not in others like Japanese or Chinese.
    4. Making Launch Decisions: Decide whether to implement a change by considering the statistical and practical significance, understanding its effect on user experience, and evaluating its overall worth.
    5. Importance of Recommendations: The ultimate goal is to make a well-judged recommendation based on the experiment's findings. The focus should be on the correctness and impact of the final recommendation to launch or not launch the change.
10. Changes over time
    1. Ramp-Up Practice: It's considered good practice to gradually ramp up an experiment, starting with a small percentage of traffic and increasing it until full launch.
    Removing Filters in Ramp-Up: During the ramp-up phase, it's important to remove any filters (like language restrictions) used in the initial testing. This helps identify any incidental impacts on users not included in the original experiment.
    2. Flattening Effect During Ramp-Up: An interesting challenge is that the effects observed initially may flatten out as the experiment is ramped up, even if they were statistically significant at first.
    3. Reasons for Non-Repeatable Effects: Non-repeatable effects can be due to seasonality, such as student behavior changes during summer vacation or shopping behavior changes around holidays like Black Friday and Cyber Monday.
    4. Using Holdbacks to Capture Seasonal Impacts: To capture these seasonal or event-driven impacts, a technique called 'holdback' is used. The primary purpose of a holdback is to have a continuous baseline against which to compare the behavior of users who are experiencing the new changes. This comparison helps isolate the impact of the change from other external factors.
    5. Novelty Effect and Change Aversion: Changes in user behavior due to novelty effects or change aversion can also impact the results as users adapt to the change. Cohort analysis is useful for understanding these changes over time.
    6. Advertisers’ Budgets Impacting Results: In cases where advertisers have budgets, the ramp-up might affect the observed impact if budgets aren't controlled properly. Consider an experiment where a website redesign leads to ads being more prominently displayed. Initially, this might result in higher engagement rates with the ads. In response, advertisers may increase their spending, leading to a short-term surge in the website's ad revenue. However, this surge might not be sustainable or reflective of the long-term impact of the redesign, as it is heavily influenced by the increased advertiser spending rather than just the design changes. This interplay between experiment-induced performance changes and advertisers' budget responses adds complexity to the analysis of experimental results. Need to control for advertiser budgets
    7. Complex Analyses for Understanding Changes: To understand and catch these effects during the experiment, complex analyses and experiment designs are needed. This includes using pre- and post-period analyses along with cohort analysis to understand how users are adapting to the change over time.
    8. Cost of engieerning maintaining the change, customer support or sales issues. Opportunities cost if you choose to launch the change relative to the reward 



​​​




