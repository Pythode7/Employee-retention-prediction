# Employee-retention-prediction

## Business scenario and problem

The HR department at Salifort Motors wants to take some initiatives to improve employee satisfaction levels at the company. They collected data from employees, but now they don’t know what to do with it. They refer to you as a data analytics professional and ask you to provide data-driven suggestions based on your understanding of the data. They have the following question: what’s likely to make the employee leave the company?

The goals in this project are to analyze the data collected by the HR department and to build a model that predicts whether or not an employee will leave the company.

If we can predict employees likely to quit, it might be possible to identify factors that contribute to their leaving. Because it is time-consuming and expensive to find, interview, and hire new employees, increasing employee retention will be beneficial to the company.

## HR dataset

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

**Boxplot showing distributions of `satisfaction_level` by tenure, comparing employees who stayed versus those who left**  

![image](https://github.com/Pythode7/Employee-retention-prediction/assets/98425039/a8c4915c-10e3-4e12-abff-b345a3d8d62d)  

_**Observations:**_   
1. Two groupes of employees who left the company:
   - Dissatisfied Short-Timers: There seems to be a correlation between low satisfaction and shorter employee tenure. This suggests a focus on improving the onboarding and early career experience for new hires.
   - Highly satisfied Medium-Timers (5-6yrs): These employees might feel they've plateaued in their current roles and seek new challenges or opportunities for professional growth that their current company might not offer.
2. Four-Year Dip: The unusually low satisfaction level among four-year employees who left warrants further investigation. Investigate potential policy changes or events occurring around the four-year mark that might have impacted this group specifically.
3. The highest satisfaction is observed among long-tenured employees (those who haven't left). Their satisfaction aligns with that of satisfied new hires who chose to stay. This suggests a positive long-term work environment for those who stay.
4. Tenure and Hierarchy: The histogram suggests a smaller number of employees with longer tenure. It's possible these are higher-ranking or higher-paid employees, potentially with more influence and satisfaction that keeps them with the company.

