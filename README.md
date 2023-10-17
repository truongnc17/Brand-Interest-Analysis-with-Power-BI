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
Meaning: dashboard describes basic information related to survey participants' demographics - age, region, gender, household income. Through this, you can get an overview of the survey participants' demographic characteristics.
Insight: 
* There is not much difference in age and income among the groups participating in the survey. These target groups all have a high average household income of ~ $104.63k and are concentrated in large city areas with a high average standard of living. In which the age group with higher income ($105.29k ~ $105.77k) is concentrated in the age groups from 14 - 20 and 30 - 40
=> Brands focus mainly on target audiences with high average household income (because there are industries where the spenders are not the direct users)
   These brands are aiming to survey many different age groups to find out which audience the brand is mainly targeting and whether it is true to the brand's target 
   customer group. signal or not.
#### 3.2 Marketing efficiency
![image](https://github.com/truongnc17/Brand-Interest-Analysis-with-Power-BI/assets/131191379/2de885f5-29c0-4a7f-aae3-51537daa50e5)
Meaning: This dashboard shows us the correlation between each brand's marketing costs and factors such as interest level, interest coefficient, age and geographical location to make an assessment of interest level. The effectiveness of the total costs spent each month.
Insight: 

