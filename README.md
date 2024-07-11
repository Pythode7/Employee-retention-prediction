# Employee-retention-prediction

## Business scenario and problem

The HR department at Salifort Motors wants to take some initiatives to improve employee satisfaction levels at the company. They collected data from employees, but now they don’t know what to do with it. They refer to you as a data analytics professional and ask you to provide data-driven suggestions based on your understanding of the data. They have the following question: what’s likely to make the employee leave the company?

The goals in this project are to analyze the data collected by the HR department and to build a model that predicts whether or not an employee will leave the company.

If we can predict employees likely to quit, it might be possible to identify factors that contribute to their leaving. Because it is time-consuming and expensive to find, interview, and hire new employees, increasing employee retention will be beneficial to the company.

### HR dataset

Variable  |Description |
-----|-----| 
satisfaction_level|Employee-reported job satisfaction level [0&ndash;1]|
last_evaluation|Score of employee's last performance review [0&ndash;1]|
number_project|Number of projects employee contributes to|
average_monthly_hours|Average number of hours employee worked per month|
time_spend_company|How long the employee has been with the company (years)
Work_accident|Whether or not the employee experienced an accident while at work
left|Whether or not the employee left the company
promotion_last_5years|Whether or not the employee was promoted in the last 5 years
Department|The employee's department
salary|The employee's salary (U.S. dollars)

### Initial EDA and data cleaning

- In this dataset, there are 14,999 rows, 10 columns, no missing values, and 3008 duplicates (~20% of the data). 
- All the misspelled variable names were corrected and all column names were converted into lower case and standardized in snake_case.   

**Boxplot showing `average_monthly_hours` distributions for `number_project`, comparing employees who stayed versus those who left**  

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/79424bba-6d29-460f-accc-11994acf67d7)  

_**Observations:**_
1. Two groupes of employees who left the company:
   - Employees who worked significantly fewer hours compared to colleagues with similar project loads. This group may include individuals who were terminated or those who submitted notice and had their hours reduced during their exit process.
   - Employees who worked considerably more hours than their peers with similar project assignments. This potentially indicates employees who chose to leave (potentially due to high workload) and may have been significant contributors to their projects.
     
2. The ratio of left to stayed is significantly lower for employees managing 3-4 projects compared to other project loads, suggesting a potential sweet spot of 3-4 projects for optimal employee retention.
3. Employees assigned seven projects all departed the company. Notably, the interquartile range (IQR) of their work hours (255-295 hours/month) is significantly higher than any other group.
4. Assume a work week of 40 hours and two weeks of vacation per year, then the expected working hours per month of employees working Monday–Friday = 50 weeks * 40 hours per week / 12 months = 166.67 hours per month. Compare each employee's work hours for the chosen period with the expected monthly working hours (166.67). Employees who significantly exceed this threshold (e.g., by more than 10-15%) could be flagged as potentially overworked.

**Scatterplot of `average_monthly_hours` versus `satisfaction_level`, comparing employees who stayed versus those who left**   

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/5fe9631b-4dc5-4d4f-a569-a96dd0c4be5d)  

_**Observations:**_  
- High Work Hours, Low Satisfaction (240-315 hours/month): A significant number of employees who left  worked extremely long hours, exceeding 75 hours per week for a year (based on a 40-hour work week assumption). This group also reported very low satisfaction levels (close to zero), suggesting a potential correlation between excessive workload and dissatisfaction.
- Normal Work Hours, Moderate Satisfaction (unknown range):  Another group of departing employees had typical work hours. While their satisfaction wasn't as low as the high-hour group, it was still only around 0.4. It's difficult to pinpoint the exact reasons for their departure without further data.
- Moderate Work Hours, High Satisfaction (210-280 hours/month):  In contrast, there's a group of employees who worked moderate hours (210-280 hours/month) and reported high satisfaction levels (0.7-0.9). This suggests that for some employees, maintaining a work-life balance with moderate hours is associated with greater job satisfaction.

**Boxplot showing distributions of `satisfaction_level` by tenure, comparing employees who stayed versus those who left**  

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/a8c4915c-10e3-4e12-abff-b345a3d8d62d)  

_**Observations:**_   
1. Two groupes of employees who left the company:
   - Dissatisfied Short-Timers: There seems to be a correlation between low satisfaction and shorter employee tenure. This suggests a focus on improving the onboarding and early career experience for new hires.
   - Highly satisfied Medium-Timers (5-6yrs): These employees might feel they've plateaued in their current roles and seek new challenges or opportunities for professional growth that their current company might not offer. These
2. Four-Year Dip: The unusually low satisfaction level among four-year employees who left warrants further investigation. Investigate potential policy changes or events occurring around the four-year mark that might have impacted this group specifically.
3. The highest satisfaction is observed among long-tenured employees (those who haven't left). Their satisfaction aligns with that of satisfied new hires who chose to stay. This suggests a positive long-term work environment for those who stay.
4. Tenure and Hierarchy: The histogram suggests a smaller number of employees with longer tenure. It's possible these are higher-ranking or higher-paid employees, potentially with more influence and satisfaction that keeps them with the company.

**Scatterplots of `average_monthly_hours` versus `last_evaluation` and `average_monthly_hours` versus `promotion_last_5years`**   

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/3b4f4797-3088-472c-a88d-79e517baae85)    

- Overworked Top Performers: Scatterplot shows high performers leaving despite strong evaluations, suggesting potential burnout due to excessive hours. Long hours aren't a guarantee of good performance.
- Company Culture Shift Needed: Majority of employees work well above expected hours. Consider promoting work-life balance initiatives to address potential overwork and improve retention.

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/3b9ba77a-d298-4a9e-81b9-d4ba3f14e8b4)  

- Promotion and Retention: Few employees got promoted and those who promoted in the last five years have very low turnover, suggesting promotion is a potential factor in employee retention. These might explain why employees who have worked for 5-6 years with high satisfaction left the company.
4. Long Hours and Departures: Employees who worked the most hours were rarely promoted and all departing employees belonged to this group. This suggests a possible link between overwork and employee dissatisfaction leading to departures.

#### Insights   
Analysis reveals a correlation between longer work hours and higher departure rates. Additionally, employees who left often managed multiple projects. These observations suggest a possible risk of burnout among employees with heavy workloads. The data shows a lower turnover rate among employees with longer tenure (more than six years).   

### Models building and evaluatioin   

#### Logistic Regression   
**Feature engeering and model assumption test**   
- Encode the `salary` column as an ordinal numeric category
- Dummy encode the `department` column
- Remove the outliers that were identified earlier since logistic regression is quite sensitive to outliers
- Heatmap showed no severe multicolliniearity among the selected features

#### Tree-based Models: Decision Tree, Random Forest
- Construct a decision tree model and a random forest model and set up cross-validated grid-search to exhuastively search for the best model parameters.
- Feature engineering was applied to reduce the chance of data leakage:
   - Employee satisfaction data may be incomplete, hindering definitive links to departures.
   - Average monthly hours might be skewed by pre-departure changes in employee work patterns.

#### Evaluation scores for


  


