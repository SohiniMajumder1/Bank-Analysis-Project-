# Bank Loan Analysis-Project

Power BI Dashboard Link:- https://app.powerbi.com/links/Yx_yluj8Aj?ctid=a244f90e-beae-4adc-a8cb-4c0c34466fd2&pbi_source=linkShare


Tableau Link :- https://public.tableau.com/app/profile/sohini.majumder/viz/BankLoanAnalysisDashboard_17360956037580/Dashboard2?publish=yes



Tools Used

1.Power BI: For data visualization and dashboard creation.
2.Power Query: For data transformation and cleaning.
3.MySQL: For data storage and query execution.
4.Tableau:For data visualization and dashboard creation.

Objective: To analyze the growth in bank loans over the years and identify patterns, trends, and actionable insights to drive business decisions.

Key KPIs Analyzed

1.Year-wise Loan Amount Statistics.
2.Grade and Subgrade-wise Revolving Balance.
3.Total Payment for Verified vs. Non-Verified Status.
4.State-wise and Month-wise Loan Status.
5.Homeownership vs. Last Payment Date Stats.
6.Purpose of Loans.
7.Average Interest Rate and Debt-to-Income (DTI) Ratio.


Steps to Create the Dashboard

1. Data Preparation:

Import datasets (.csv and .xlsx) with 39k rows each into Power Query.
Clean and transform data: Handle missing values, unify date formats, and create calculated columns (e.g., year, month).
Load transformed data into Power BI.

2.Data Modeling:

Build relationships between tables in the DataModel.
Create measures for key metrics (e.g., total loan amount, average DTI ratio).
Use DAX for advanced calculations (e.g., year-on-year growth rates).
Visualizations:

3.Design visuals for each KPI:

Line Chart: Year-wise loan growth.
Bar Charts: Grade and subgrade-wise revolving balances.
Pie Chart: Verified vs. Non-Verified payment distribution.
Map: State-wise loan distribution.
Calendar Visual: Month-wise loan activity.
Apply slicers and filters (e.g., year, state, grade) for interactive analysis.

4.Dashboard Customization:

Arrange visuals for clarity and aesthetics.
Add tooltips and annotations to highlight insights.
Configure navigation (e.g., drill-through to detailed views).
Publish and Share:

5.Publish the dashboard to Power BI Service.
Share with stakeholders via live links or embedded reports.


Key Findings

1.Year-wise Loan Growth:Gradual increase year-over-year, with significant spikes in 2010 and 2011.

2.Grade and Subgrade Balances:Highest revolving balances in grades B and A; lowest in G.

3.Loan Verification: Verified loans: 41.12% of payments ($153.54M) with a 14.25 DTI ratio.Non-verified loans: 58.88% of payments ($219.89M) with a 13 DTI ratio.

4.State and Month Insights:Top states: California, New York, Florida.Most loans disbursed in December; least in January.
Loan statuses: Fully Paid (32,950), Charged Off (5,627), Current (1,140).

5.Homeownership and Loan Payments:Highest category: Rent (18,847 loans), followed by Mortgage (17,645 loans).

Other Metrics:

6.Average interest rate: 12%.
7.Average DTI ratio: 13.
8.Top loan purpose: Debt consolidation; least popular: Renewable energy.

Key Benefits

1.Improved Decision-Making:
Understand year-wise growth and key trends in loan distribution.
Identify high-performing states and borrower segments.

2.Enhanced Targeting:
Focus on verified loans to improve payment consistency.
Tailor strategies for under-represented grades and loan purposes.

3.Operational Efficiency:
Optimize resources in high-potential states.
Monitor grades and subgrades to balance risk and returns.

Suggestions for Business Growth

1.Increase loan verification to reduce defaults.
2.Expand operations to high-potential states.
3.Prioritize high-performing borrowers based on grades and payment history.
4.Develop strategies for under-represented grades to boost inclusivity.
5.Leverage insights from top-performing states to replicate success elsewhere.
6.Implement early warning systems for risk mitigation.


Attaching the MySQL Queries :- 

use bank_analytics;

select * from finan_1;	
select * from finance_2;

-- KPI 1 :- Year wise loan amount Stats

select year(issue_d) as issue_year, concat(format(sum(loan_amnt)/1000000,0),'M') as loan_amt 
from finan_1
group by year(issue_d)
order by year(issue_d);

-- KPI 2 :- Grade and sub grade wise revol_bal
select grade,sub_grade, concat(format(sum(revol_bal)/1000000,0),'M') as revol_bal
from finan_1 join finance_2
on finan_1.id = finance_2.id
group by grade,sub_grade
order by grade;


-- KPI 3 :- Total Payment for Verified Status Vs Total Payment for Non Verified Status

select  verification_status, concat(format(sum(total_pymnt)/1000000,1),'M') as total_payment
from finan_1 join finance_2 
on finan_1.id = finance_2.id
where verification_status in ("not verified","verified")
group by verification_status
order by total_payment;

-- KPI 5 :- Home ownership Vs last payment date stats

SELECT home_ownership,count(last_pymnt_d) AS "Total"
FROM finan_1 
LEFT JOIN finance_2 
ON finan_1.id=finance_2.id
GROUP BY home_ownership;

-- KPI 4 :- Month Wise and State Wise Loan Status

Select addr_state as 'Address State',monthname(issue_d) as Month , loan_status as 'Loan Status', count(loan_status) as Total
FROM finan_1
group by addr_state,monthname(issue_d), loan_status
Order by addr_state;












