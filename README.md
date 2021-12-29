# Udacity A/B Testing Final Project: Free Trial Screener

## Experiment Design
### 1.Metric Choice
Which of the following metrics would you choose to measure for this experiment and why? For each metric you choose, indicate whether you would use it as an invariant metric or an evaluation metric. The practical significance boundary for each metric, that is, the difference that would have to be observed before that was a meaningful change for the business, is given in parentheses. All practical significance boundaries are given as absolute changes.


Any place "unique cookies" are mentioned, the uniqueness is determined by day. (That is, the same cookie visiting on different days would be counted twice.) User-ids are automatically unique since the site does not allow the same user-id to enroll twice.


- Number of cookies: That is, number of unique cookies to view the course overview page. (dmin=3000)
- Number of user-ids: That is, number of users who enroll in the free trial. (dmin=50)
- Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)
- Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)
- Gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
- Retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
- Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

#### Invariant Metrics: 
Number of Cookies, Number of Clicks, Click through probability. 
These metrics happen before the experiment starts. Therefore, it is important to split these metrics evenly. 
#### Evaluation Metrics: 
The hypothesis is that by setting the new feature, it will set expectations for students and filter out students who do not have enough time to learn. Therefore, we should choose metrics that can reflect the impact of this new feature. 

Gross Conversion: Decrease, filter out students who do not have time to study
Retention: Retention may increase, if less students who are frustrated participate the course
Net Conversion: correlated to Gross Conversion




### 2.Measuring Standard Deviation

Based on the given sample size, 5000, the other data from google sheets. 
The metric is probability, which follows the binomial distribution. 
The formula to calculate SD of binomial distribution is SQRT(mean*(1-mean)/N)
Tips: Convert N to the same units. 

- Gross Conversion: 0.0202
- Retention: 0.0549
- Net Conversion: 0.0156

### 3.Sizing
#### Number of Samples vs. Power
Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.)

I will use Gross Conversion, Retention and Net Conversion as evaluation metrics. Since those metrics are correlated with each other, I will not use Bonferroni Correction, which is too conservative in this case.

Given by, alpha = 0.05, beta = 0.2 
The link to calculate the sample size
https://www.evanmiller.org/ab-testing/sample-size.html

**Gross Conversion***:
- baseline : 0.20625
- Minimum detectable effect: 0.01 
- Sample size (click) for each group is 25835
- Two group is 25835 * 2 = 51670
- Since the unit is click and we need to calculate the overall page view. Based on the probability of click, 51670 / 0.08 = 645875
- The page views are 645875

**Retention**:
- baseline : 0.53
- Minimum detectable effect: 0.01 
- Sample size (click) for each group is 39115
- Two group is 39115 * 2 = 78230
- Since the unit is enrolled, we need to calculate the overall page view. Based on the probability of enroll, 78230 /  = 4741212
- The page views are 4741212

**Net Conversion**:
- baseline : 0.1093
- Minimum detectable effect: 0.0075 
- Sample size (click) for each group is 27411
- Two group is 27411 * 2 = 54822
- Since the unit is click and we need to calculate the overall page view. Based on the probability of click, 54822 / 0.08 = 685275
- The page views are 685275

With multiple metrics, we choose the maximum sample size, which is 4741212

#### Duration vs. Exposure
Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)

Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

- Ans: 100% traffic, 18 days

- Given the daily traffic from data, if we run the experiment with 4741212 sample size, it will take 119 days, which is a very long time to run. Therefore, we can remove the metric, Retention, and keep the other two metrics, Net Conversion and Gross Conversion, which will decrease the sample size to 685275. With this sample size, it needs 17 days to run with 100% traffic and 35 days to run with 50% traffic. Since this feature is not a big change that will influence the number of people who enroll in the course, I think it is not very risky to run the experiment with 100% traffic. And 18 days are more acceptable than other numbers of days.

## Experiment Analysis
### 1.Sanity Checks
For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)

For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass.

The mean probability of distribution should be 0.5, based on the SD and Z score, 1.96 , we can get the confidence level. If the confidence level includes the Observed value, then it is passed.

SD = Sqrt(mean * (1-mean) / (N1+N2)
![Sanity Check.png](https://github.com/ShuangyangWu1/test-/blob/81c7f40cef51ab24e42815feff8292a1ebc2f9d8/Sanity%20Check.png)




## Result Analysis
### 1.Effect Size Tests
For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

**Significance definitions**
A metric is statistically significant if the confidence interval does not include 0 (that is, you can be confident there was a change), and it is practically significant if the confidence interval does not include the practical significance boundary (that is, you can be confident there is a change that matters to the business.)

Metrics: Gross Conversion and Net Conversion
Since the metric is measured the performance of the past 14 days, I will ignore the data of last 14 days .

**Gross Conversion**:
- Probability of Control Group: Sum(enroll) / Sum(clicks) = 0.2189
- Probability of Experiment Group: Sum(enroll) / Sum(clicks) = 0.1983
- Diff: P(Exp) - P(Cont) = 0.1983- 0.2189 = -0.0206
- P(pool): [Cont(enroll) + Exp(enroll)] / [Cont(clicks) + Exp(clicks)] = 0.2086
- SD (SE): Sqrt(pool * (1-pool) * [1/Cont(clicks) + 1/(Exp(clicks)] = 0.0044
- Margin: Z * SD = 1.96 *SD =  0.0086
- Lower: -0.0291
- Upper: -0.0120
Since the confidence interval does not include 0, it is statistically significant.
The confidence interval does not include practical significance, it is practically significant

**Net Conversion**:
- P(cont): 0.1176
- P(exp): 0.1127
- Diff: -0.0049
- P(pool): 0.1151
- SE: 0.0034
- margin: 0.0067
- Lower: -0.0116
- Upper: 0.0019
Neither statistically significant Nor practically significant

### 2.Sign Tests
For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)


Calculate the number of metrics that experiment group is greater than control group
![Sign Test](https://github.com/ShuangyangWu1/test-/blob/f745643c7a86ba69885d636be9ffbd1900500a37/Sign%20Test.png)

Use this link https://www.graphpad.com/quickcalcs/binomial2/ to calculate p-value

## Summary
State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

I did not use the Bonferroni Correction since those metrics are correlated to each other. The Bonferroni correction method is too conservative in this case that increase the chances that accept the null hypothesis, which may impact the result of experiment. By doing the sanity check, the invariants metrics are all splited evenly. Therefore, we can dig deeper into the result analysis. The Gross Conversion is statistically significantly and practically significantly, but, Net Conversion is neither statistically significantly nor practically significantly. 

## Recommendation

Based on the result that I get above, I will suggest not to lauch since the result shows this experiment will not make the increase in the net conversion


Reference:
https://github.com/shubhamlal11/Udacity-AB-Testing-Final-Project/blob/master/README.md

http://cyfz11.com/2020/03/22/Final%20Project/

https://nancyyanyu.github.io/posts/8fdfc10f/
