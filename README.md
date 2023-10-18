# Brand-Interest-Analysis-with-Power-BI

### 1. Data description
#### 1.1 Context
As a brand management specialist, tasks include: collecting customer opinions related to factors affecting brand preference/choice and vice versa, comparing correlation with cost. marketing costs, thereby evaluating the effectiveness of marketing costs and finding insights that lead to decisions about customers' brand perception.
#### 1.2 Dataset
The report is based on a survey of product purchasing and usage experience opinions of 10,000 new customers in several US states in 2019 of some major brands in the following fields: Mercedes-Benz, Toyota, Amazon, Disney, McDonald's, Coca-Cola, Apple, Samsung, Google, Microsoft

- Dataset includes 1 table with 20 fields including main components:
 * Customer information: demographic information (age, gender, income, location)
 * Brand
 * Indicators related to the level of customer interest: Interest Coefficient, Interest Level
 * Marketing expense
 * Customer survey questions:
   * What is your first reaction to the brand products?
   * How would you rate the quality of this brand of products?
   * How innovative is this brand?
   * When you think about the brand, do you think of it as something you need or don't need?
   * How would you rate the value for money of their products?
   * How likely would you be to try new products of this brand?
   * How likely are you to replace a current brand with this brand?
   * How likely is it that you would recommend new products from this brand to a friend or colleague?
#### 1.2 Analysis
* Marketing efficiency
* Customer Satisfaction
* Net Promoter Score
* Customer Sentiment
### 2. Possesing 
#### 2.1 Clean and Transform Data
S1: Load data
S2: Normalize data; Split and transform dataset into dim, fact tables, add weight and measure value columns; Change the data format accordingly
S3: Create additional Date table
#### Data modeling below
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/9e625f4b-8abd-4f7e-9b71-5383a55cae7c)
#### 2.2 Create DAX formulas
##### * Caculate Customer Satisfaction Score
S1: Score x Response = SUM('CSAT Feedback'[CSAT range ])
S2: Number of response = COUNTROWS('CSAT Feedback')
S3: CSAT Score = DIVIDE([Score x Response],[Number of response],0)
##### * Caculate Percent of Customer Satisfaction Score
CSAT Percent =
VAR _CSATCount =
CALCULATE(COUNTROWS('CSAT Feedback'),FILTER('CSAT Feedback', 'CSAT
Feedback'[CSAT range ] >3)
)
VAR total = COUNTROWS('CSAT Feedback')
Return
DIVIDE(_CSATCount,total)
##### * Caculate Net Promoter Score
S1: NPS Promoters % = (CALCULATE(COUNT(General[NPS Rating]),General[NPS Rating]>=8)/COUNT(General[NPS Rating]))*100
S2: NPS Detractors % = (CALCULATE(COUNT(General[NPS Rating]),General[NPS Rating]<8)/COUNT(General[NPS Rating]))*100
S3: NPS % = [NPS Promoters %]-[NPS Detractors %]
##### * Classify Net Promoter Score
NPS = IF(General[NPS Rating]>=8,"Promoters", IF(General[NPS Rating]<7, "Detractors", "Passives"))
##### * Caculate Target Net Promoter Score Group
TargetNPSGroup = IF('Brands'[Brand (groups)] = "Automobile", 39, IF('Brands'[Brand (groups)] = "Technology", 58, (IF('Brands'[Brand (groups)] = "F&B", 41, IF('Brands'[Brand (groups)] = "Smartphone", 28, IF('Brands'[Brand (groups)] = "E-Commerce", 51, 40))))))
##### * Classify Customer Sentiment
CS rating = IF('Customer Sentiment'[CS range]>3,"Positive", IF('Customer Sentiment'[CS range]<3, "Negative", "Neutral"))
##### * Divide customers' Repurchase capabilities
Repurchase = IF(Retention[Retention rating]>2,"Certainly",IF(Retention[Retention rating]<2,"Not at all likely","Neutral"))
### 3. Data Visualizing 
#### Difficulty:
Determining the chart content, chart format, presentation layout, the clean data process also faces many difficulties due to not knowing how to handle null values and link value fields together.
#### Method: 
PDCA (Plan - Do - Check - Act) to final version.
#### 3.1 Overview
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/16ec56e7-72e4-409f-ac16-c69f8106f51d)

**Meaning**: dashboard describes basic information related to survey participants' demographics - age, region, gender, household income. Through this, you can get an overview of the survey participants' demographic characteristics.

**Insight**: 
* There is not much difference in age and income among the groups participating in the survey. These target groups all have a high average household income of ~ $104.63k and are concentrated in large city areas with a high average standard of living. In which the age group with higher income ($105.29k ~ $105.77k) is concentrated in the age groups from 14 - 20 and 30 - 40
=> Brands focus mainly on target audiences with high average household income (because there are industries where the spenders are not the direct users)
   These brands are aiming to survey many different age groups to find out which audience the brand is mainly targeting and whether it is true to the brand's target 
   customer group. signal or not.
#### 3.2 Marketing efficiency
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/2de885f5-29c0-4a7f-aae3-51537daa50e5)

**Meaning**: This dashboard shows us the correlation between each brand's marketing costs and factors such as interest level, interest coefficient, age and geographical location to make an assessment of interest level. The effectiveness of the total costs spent each month.

**Insight**: 
#### Chart 1
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/a74098a6-b784-40ff-aabf-4b6d7a925340)

**Meaning**: shows the correlation between marketing costs and brand love level by month to evaluate the effectiveness of costs spent compared to the level of love gain. (Each brand will have differences)

**Insight**: it can be seen that the interest level of brands (in the same industry group) does not change/difference significantly over months. The average popularity level is relatively even. For example: samsung > apple is about 0.27 and microsoft > google around 1.03.
=> We can see a balance in the level of love between brands in the same industry group among consumers of different ages. And even if marketing costs increase, the level of popularity does not have a corresponding increase.
  It is necessary to have an optimal strategy in each stage with the goal of converting customers from brand awareness to love.
For example : F&B Industry
Characteristics: high level of substitution, low loyalty, low purchase involvement due to a small percentage of total spending. If you want consumers to remember and use the product and thereby gain love, you need frequent and creative appearances to remind them.
Image 1: McDonald
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/febb14f4-fb88-4257-9208-746185ea0e44)
Image 2: Coca Cola
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/a84e607f-e9a0-4508-9536-faf7d5d7dbaf)
It can be seen that McDonald's has a fairly even level of marketing spending throughout the months of the year and achieves a higher average popularity level than Cocacola. While Cocacola's average marketing costs are higher than McDonald's.
=> Marketing costs that are spread evenly over each month will bring higher conversion efficiency.
#### Chart 2
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/84479009-4b02-4735-a036-b5359c4c6542)

**Meaning**: correlation between marketing costs and customer interest coefficient by each age group. The interest factor is often used to evaluate the likelihood of a customer making a purchase or taking a desired action and is calculated by the customer's level of interaction with a brand's marketing materials.

**Insight**:
* It can be seen that the interest coefficient gradually decreases with increasing age. This is quite understandable because young people tend to be more interested in and approach brands than older people.
* Big brands have captured this interest, so they have focused on investing in marketing activities for older age groups to expand their customer base and increase the interest of these age groups (namely, age groups 41 and over)
#### Chart 3
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/d67918f0-cf0c-4a8f-9a20-1d6e307c250e)

**Meaning**: Average popularity level by state - evaluates brand interest trends by geographical location.

**Insight**:
* We can easily see that the level of popularity is distributed quite evenly between states. The level of love between states has a relatively small margin, however we can see that DC is the state with the highest level of brand love followed by PA, DE, NY and finally VA
* It can be concluded with this data set that the geographical location factor does not affect too much the level of brand preference of customers (it can be assumed that the sales policies of brands between states are mostly no difference)
#### 3.3 CSAT
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/d34da9a6-ad65-40e3-ae40-77b9cea3573e)
CSAT is an index used to measure the level of customer satisfaction during the product or service experience. The CSAT index is often measured with short questions and is often placed at the end of the survey to collect feedback on influencing factors to customers' product usage experience for businesses on pre-built scales.
Meaning: dashboard shows the correlation of CSAT index with income groups, age groups and the ability of customers to continue buying and using products of that brand.
#### Chart 1
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/06525fc7-49c8-4df3-9682-b26e99c97fcc)

**Meaning**: Correlation between customer income and CSAT index.

**Insight**:
* Looking at the chart, we can confirm that the middle income group tends to be more satisfied with the brand than the high income group.
* The only two industries in the high-income group with a higher CSAT index than the average income group are E-commerce and Smartphones.
#### Chart 2
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/e7b199f9-458c-4689-a7f5-ab3cb6c0deea)

**Meaning**: Total number of participants in the CSAT assessment by age group

**Insight**:
* With each CSAT scale, we can know the number of people in each age group evaluating opinions
  **Excellent level**: The age group 60 and over receives the most reviews
  **Above average level**: Age group 31-40 rated the most
  **Average level**: Age group 41-50 rated the most
  **Below average**: Age group 0-20 rated the most
  **Poor level**: The age group 21-30 is rated the most
* From here it can be concluded that younger age groups tend to evaluate more harshly than older age groups and always want more from the brand's products.
#### Chart 3
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/4e777d7d-bcdf-4f90-904c-1c53144b4d44)

**Meaning**: Number of reviews with a desire to continue using the brand's products

**Insight**:
* Although the level of customer satisfaction is not rated too high, nearly half of the number of reviews still choose to continue to trust the brand (49,820/100,000 reviews of 10,000 participants with 10 brands).
* More than half of the remaining survey reviews are still hesitant or choose not to continue using the brand's products. This requires brands to come up with policies to retain and build customer trust in their products.
#### 3.4 NPS
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/d04f5981-6502-4454-8a85-060a3e05a15a)
Net Promoter Score (NPS) is a measurement of customer satisfaction and loyalty derived from asking customers how likely they are to recommend your product or service to others. NPS is the typical answer to the question "On a scale of 0 to 10, how likely are you to recommend it to a friend?" in survey.
Customers capable of recommending products (reviews) can come from people who have/have never participated in shopping/using the brand's products, in which people who have participated in using and recommending can have The level of reuse and conversion is higher and the referral will be more reputable.
The answers to this question can be classified into three groups:
* Promotor (9-10)
* Passive (7-8)
* Detractor (0-6)
NPS is calculated by the difference between %Promoter and %Detractor.
Dashboard provides information about the correlation between NPS index and interest level (Interest Level) and marketing costs. In addition, there is an NPS index by survey participant segment.
#### Chart 1
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/042948aa-9b6d-4055-85c5-6e101514b28c)

**Meaning**: NPS index and NPS Target Index

**Insight**: 
* The chart shows the NPS index of each brand in each industry, but all industries are negative and much smaller than the target NPS of the entire industry.
* This shows that the industry has more detractors than promoters: customers who have experienced the product do not want to recommend the brand's products to acquaintances.
#### Chart 2
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/8709e6b3-85ac-4f0e-8a07-35310b833e25)

**Meaning**: the level of favorability compared to the customer's recommendation rate, thereby deducing whether the customer's love of the product is proportional to the customer's ability to recommend that brand to others.

**Insight**:
* Because all industries recorded a negative NPS index, it can be affirmed that just because a customer loves a product does not mean that that customer will recommend the product to others.
* Some brands record low product love and low referral rates like Google.
* However, the remaining brands, although recording a high level of popularity, have a low referral rate: Microsoft, Samsung.
=> It can be seen that although customers have a high level of love, they are not really ready to share the brand with other consumers. There are many factors leading to this problem, for example, for the F&B industry, which has a high frequency of appearing in consumers' daily conversations, such as McDonald's or Cocacola because it is so familiar and is a fast-moving consumer goods industry. will be more likely to spread word - of - mouth than other specific industries. As for Google and Microsoft, they are two technology companies with a large range of awareness and consumption but have a lower referral rate because the context referring to these companies' products will be more specific and less than other industries. other products and at the same time, these two technology companies do not have too many alternatives, so consumers will have less referrals.
  The large difference in product preference and recommendation rate may also be because they like the product but are still not completely confident in recommending it.So, depending on each industry, brands need to have appropriate marketing strategies, optimizing the product's usage context or reminding consumers in each different stage.
#### Chart 3
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/e1c2fd49-d0bc-4217-a9b6-ac6133e13c7f)

**Meaning**: The chart shows the correlation between marketing costs and NPS ratio. If the NPS rate is high, the brand will reduce part of its marketing costs, but it must have a large referral rate to offset the overall costs because in reality it is difficult to measure and control. Control whether customers recommend the product or not regardless of good/bad usage experience.

**Insight**: Most brands have high marketing costs but low NPS rates, especially Google.This shows that brands need to pay more attention to budget management to improve customer referral rates with a reasonable marketing cost.
#### Chart 4
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/570cb684-4c94-4c2e-a1c9-9f14a0f8d075)

**Meaning**: Percentage of Detractor, Passive, Promoter among all survey participants along with percentage by gender.

**Insight**:
* The ratio of men and women in each category is quite even (almost 50-50).
* Detractor percentage is extremely high (nearly 65%) while Promoter percentage is low (nearly
28%) results in a negative NPS score.
=> Customer experience is poor. Customers can easily abandon products and brands when they find better products. Businesses need to research and find problems to improve.
#### Chart 5
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/20510d42-a19f-4687-95cd-2049b497be3e)

**Meaning**: Detractor, Passive and Promoter distribution among all participants divided by age group

**Insight**: Age groups have an even division of Detractors, Passives and Promoters, Detractors (over 1000) have the largest number followed by Promoters (under 5000) and Passives.
#### 3.5 Customer Sentiment
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/5e0d59de-71bb-4897-b880-c422dce7f9fb)
Customer Sentiment (CS) or Customer Sentiment is a metric that businesses use to measure how customers think and feel about their brand. This metric shows how customers feel about your brand. It tells you how to feel. The customer's overall emotion — based on the interaction with your brand at a particular point in the customer's journey — is positive, negative or neutral.
Dashboard provides information about the overall level of satisfaction or feelings of survey participants with businesses' products, divided by age group, with a statistical table of each person's total satisfaction level and a graphical chart. Shows correlations in customer sentiment across firms and industries.
#### Chart 1
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/797b44fa-f2cc-4dcb-a1a6-119fd96618bd)

**Meaning**: Number of responses according to the consumer sentiment rating scale (Positive - Positive, Neutral - Neutral, Negative - Negative)

**Insight**:
* The two levels of Positive and Negative reviews record a fairly balanced proportion, which shows that customers' attitudes toward the brand's products are opposing each other. (40.12% for Negative and 40.06% for Positive)
* A small number of respondents remained neutral towards the brand. (19.82%)
* Requires brands to grasp consumer psychology to increase customer satisfaction with products through price/quality/customer care service policies to reduce the proportion of customers 2 levels Negative and Neutral.
#### Chart 2
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/17ba6291-6899-41cb-8006-f495403fc18b)

**Meaning**: Distribution of psychological level by age group of all survey participants

**Insight**:
* The 3 psychological groups Positive, Neutral, Negative have a fairly similar division between age groups with the largest number of Negative and Positive reactions (both over 6000), with the fewest Neutral reactions (about 3000). ).
* The group under 20 years old and the 41-50 year old group showed higher negative reactions than Positive reactions. While other age groups showed the opposite reaction.
=> Businesses should develop more marketing campaigns, grasp customer psychology, reduce the amount of negative reactions, keep the Positive-Negative coefficient positive, especially focusing on the group under 20 years old and the group 41-50
#### Chart 3
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/9b5c14c3-689d-497d-9417-978276f07954)

**Meaning**: Customer information, income and total CS Rating. The purpose is to evaluate the correlation between total income and total CS Rating.

**Insight**: There is not much correlation (positive or negative) between income and total CS Rating. Thereby it can be affirmed that high income does not mean high CS Rating.
=> Instead of focusing too much on customer income groups, brands need to improve product quality and actively listen to customers' opinions to increase CS Rating.
#### Chart 4
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/02b688f5-75b5-40ef-961b-3692d3edbf76)

**Meaning**: Correlation between level of brand interest and first reaction when receiving the product

**Insight**: Companies with Neutral, Positive, Negative lines are mainly around 100k Google and McDonald's are two companies with almost the same amount of Neutral, Negative, Positive responses.
