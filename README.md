
## Data Source:Dataflow

### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement
Loan Default Dataset Description

The Loan Default Dataset contains information about borrowers who have applied for loans, along with details about their financial status, loan characteristics, and repayment behavior.  

There is a lack of a centralized, interactive tool to monitor lending health and applicant risk. Specifically, the institution faces three core challenges:
​Risk Identification: High default rates are observed in specific categories (e.g., Unemployed and Part-time workers), yet the correlation between these groups and their average income or loan purpose is not fully mapped.  
​
Demographic Shifts: Loan distribution is uneven across age groups (Adults vs. Seniors vs. Teens) and credit scores (Low to High). The institution cannot easily see how marital status or income brackets influence total loan volume and repayment capability.
​Volatility in Performance: Historical data shows significant volatility in both YOY Loan Amount Change and Default Rates (notably peaks in 2015-2016). Without a visual breakdown, it is difficult to determine if these spikes are due to external economic factors or internal lending to specific risk bins.
​Objective
​The goal of this Power BI project is to develop a multi-dimensional dashboard that enables stakeholders to:
​
Analyze Default Patterns: Correlate employment types and education levels with default percentages.
​Profile the Ideal Applicant: Identify which demographic "bins" (e.g., Middle-Aged Adults with High Credit) contribute most to the portfolio.
​Forecast & Mitigate Risk: Use YOY trend analysis to identify periods of high default and adjust lending criteria for specific income brackets and employment types to ensure long-term portfolio stability.
​Key Metrics Tracked
​Performance: YOY Loan Amount Change, Average Income by Employment Type.
​Risk: Default Rate (%) by Year/Employment, Loan Amount by Credit Score Bins.
​Demographics: Median Loan Amount by Age Group, Marital Status, and Education Type.
### Steps followed 

- Step 1 : Downloading, Installing & Configuring Standard Mode Gateway
- Step 2 : Importing Data to SQL Server
- Step 3 :Creating the Dataflow using Power BI Service
- Step 4 : Importing Data into Power BI Desktop from dataflow
- Step 5 : Data Types & Profiling in Power Query Editor


- Step 6 : Create new measure as Loan Amount by Purpose (DAX Used : SUMX, FILTER, NOT, ISBLANK)

Following DAX expression was written:


        Loan Amount by Purpose = SUMX(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanAmount]))),'Loan_default'[LoanAmount])

A Line chart was used to represent Loan Amount by purpose:

![Image](https://github.com/user-attachments/assets/c6604009-7fa9-4eb8-8c52-f0516a1ce3e5)


- Step 7 : Create new measure as Average Income by Employment Type (DAX Used : CALCULATE, AVERAGE & ALLEXCEPT)

Following DAX expression was written:


        Average Income by Employment type = CALCULATE(AVERAGE('Loan_default'[Income]),ALLEXCEPT('Loan_default',Loan_default[EmploymentType]))

A Line chart was used to represent Average Income by Employment Type :

![Image](https://github.com/user-attachments/assets/7bcfb90a-ad06-488a-961f-317acb7e61d7)


- Step 8 :Create new measure as Default Rate by Employment type(DAX : ALL,ALLEXCEPT,COUNTROWS,DIVIDE,FILTER etc)

Following DAX expression was written:


        Default Rate by Employment Type = 
        VAR totalrecord= COUNTROWS(ALL('Loan_default'))
        VAR Defaultcase= COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE()))

        RETURN
        CALCULATE(DIVIDE(Defaultcase,totalrecord),ALLEXCEPT('Loan_default','Loan_default'[EmploymentType]))*100

A Line chart was used to represent Default Rate by Employment type:

![Image](https://github.com/user-attachments/assets/dbc8e791-9978-4147-b26f-509371681111)

- Step 9 :Create new measure as Average Loan Amount by Age Groups (DAX Used : AVERAGE, AVERAGEX & VALUES)

Following DAX expression was written:


        Average Loan Amount by Age Group = 
        AVERAGEX(VALUES('Loan_default'[Age group]),
        AVERAGE('Loan_default'[LoanAmount]))

A Line chart was used to represent  Average Loan Amount by Age Groups:

![Image](https://github.com/user-attachments/assets/c694e982-8b52-4dad-873d-65d6c0f828e6)

- Step 10 : Create new measure as  Default Rate by Year (DAX Used :CALCULATE, COUNTROWS, ALLEXCEPT, FILTER, DIVIDE)

Following DAX expression was written:

        Default Rate by Year = 
        VAR totalloans=
               CALCULATE(COUNTROWS('Loan_default'),
               ALLEXCEPT('Loan_default',Loan_default[Year]))

        VAR default = CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),ALLEXCEPT('Loan_default',Loan_default[Year]))

        RETURN
        DIVIDE(default,totalloans)*100

A Line chart was used to represent Default Rate by Year:


![Image](https://github.com/user-attachments/assets/eabbd554-3336-4473-8c8b-8ec03b464de5)

- Step 11 :  Median calculation & validation using MEDIANX DAX Function

- Step 12 :Create a  new measureas as Median Loan Amount by credit score category

Following DAX expression was written:

        Median by Credit score bins = 
        MEDIANX('Loan_default','Loan_default'[LoanAmount])

 A Line chart was used to represent Median Loan Amount by credit score category:

![Image](https://github.com/user-attachments/assets/0ac0f44e-85e3-452c-9b6f-848c6e3df7df)


- Step 13 : Adding Donut Chart to show Average Loan Amount by Age Group & Marital Status

Following DAX expression was written :

        Average Loan Amt (High Credit) = 
        AVERAGEX(FILTER('Loan_default','Loan_default'[Credit Score Bins]="High"),'Loan_default'[LoanAmount])

![Image](https://github.com/user-attachments/assets/c57e6faa-9e32-4b8f-a2ab-d6f1c3bce147)

- Step 14 : Create a new measure as Loan(Adults) by credit categories (DAX Used : CALCULATE,AVERAGEX,SUM)
        
Following DAX expression was written:

        Total Loan (Credit Bins) = 
        CALCULATE(SUM('Loan_default'[LoanAmount]),'Loan_default'[Age group]="Adults",ALLEXCEPT('Loan_default','Loan_default'[Age],Loan_default[Age group],'Loan_default'[CreditScore],'Loan_default'[Credit Score Bins]))

A Line chart was used to represent Loan(Adults) by credit categories:

![Image](https://github.com/user-attachments/assets/7002c31b-de99-4cbc-97a8-51565f2e02ff)

- Step 15 : New measure was created to find Total Loan (Middle Age Adults) by have Mortgage/Dependents

Following DAX expression was written:

        Total Loan (Middle age adult) = 
        SUMX(FILTER('Loan_default','Loan_default'[Age group]="Middle Age Adult"),'Loan_default'[LoanAmount])

A clusterad column chart was used to representTotal Loan (Middle Age Adults) by have Mortgage/Dependents

![Image](https://github.com/user-attachments/assets/1cbef3e2-f037-4d85-843c-360e347ec0a4)
        
 - Step 16 :New measure was created to find Loans by education type

Following DAX expression was written:

         Loan by Education Type = 
        COUNTROWS(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanID]))))

A Line chart was used to represent Loans by education type:

![Image](https://github.com/user-attachments/assets/1f0e81e5-4773-4960-92e5-91de6ac9d34c)

- Step 17 :Creating the YOY Loan amount DAX Measure
- 
Following DAX expression was written:

        YTD Loan Amount = 
        CALCULATE(SUM('Loan_default'[LoanAmount]),DATESYTD('Loan_default'[Loan Date (DD/MM/YYYY)]),ALLEXCEPT('Loan_default','Loan_default'[Credit Score Bins],Loan_default[MaritalStatus]))

A Line chart was used to represent YTD Loan Amount:

![Image](https://github.com/user-attachments/assets/f0481b69-8760-408e-80c2-d771bd23e180)

- Step 18 : Creating YOY Default Loans change DAX Measure

Following DAX expression was written: 

        YOY Default Loan Change = 
        DIVIDE(
    CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan Date (DD/MM/YYYY)])))

    ,CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan Date (DD/MM/YYYY)]))-1),0)*100

A Line chart was used to represent YOY Default Loan Change:

![Image](https://github.com/user-attachments/assets/2f94402a-ebf6-4270-99eb-67654708137a)

- Step 19 : Adding Ribbon Chart to show Loan Amount by Credit Score Bins and MaritalStatus

![Image](https://github.com/user-attachments/assets/ec9ee99b-f8e0-439b-af3c-9f8975abe1c4)

- Step 20 :Adding Decomposition Tree (DAX Used : SWITCH Function)

![Image](https://github.com/user-attachments/assets/434cc87b-eee4-4b6d-9970-f84c25351ebe)



# Report Snapshot (Power BI DESKTOP)
- Loan Default & Overview

![Image](https://github.com/user-attachments/assets/4dff7060-fee3-43a5-8e77-bb55a2faabb1)



 # Report Snapshot (Power BI DESKTOP)
- Applicant Demographics & Financial Profile
 
![Image](https://github.com/user-attachments/assets/59006a8a-22cd-452e-8da7-61ebb673e33e)


# Report Snapshot (Power BI DESKTOP)

- Financial Risk Metrics

![Image](https://github.com/user-attachments/assets/f6608c7a-7d20-472c-8d45-6955bcf40d2d)


# Insights
- Insights of Loan Amount by Purpose
1) Loan Amount by Home Purpose = 6545M
2)  Loan Amount by Business Purpose =  6522M
3)  Loan Amount by Education Purpose = 6511M
4)  Loan Amount by Auto Purpose = 6501M
5)  Loan Amount by Others = 6498M

- Insights of Average Income by Employment Type
1) Average Income by Full-time Employment = 82890
2) Average Income by Self-Emoloyment = 82447
3) Average Income by Part-time Employment = 82389
4) Average Income by Unemployed = 82272 













