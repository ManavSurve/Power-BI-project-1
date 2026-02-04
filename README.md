
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

DAX Function:


        Loan Amount by Purpose = SUMX(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanAmount]))),'Loan_default'[LoanAmount])

A Line chart was used to represent Loan Amount by purpose:

(https://github.com/user-attachments/assets/eb7093c4-e1fe-43a7-9742-240d1ae57914)


- Step 7 : Create new measure as Average Income by Employment Type (DAX Used : CALCULATE, AVERAGE & ALLEXCEPT)

DAX Function:


        Average Income by Employment type = CALCULATE(AVERAGE('Loan_default'[Income]),ALLEXCEPT('Loan_default',Loan_default[EmploymentType]))

A Line chart was used to represent Average Income by Employment Type :

https://github.com/user-attachments/assets/aecf04ea-b547-4764-a04d-d79ec64757d8


- Step 8 :Create new measure as Default Rate by Employment type(DAX : ALL,ALLEXCEPT,COUNTROWS,DIVIDE,FILTER etc)

DAX Function:


        Default Rate by Employment Type = 
        VAR totalrecord= COUNTROWS(ALL('Loan_default'))
        VAR Defaultcase= COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE()))

        RETURN
        CALCULATE(DIVIDE(Defaultcase,totalrecord),ALLEXCEPT('Loan_default','Loan_default'[EmploymentType]))*100

A Line chart was used to represent Default Rate by Employment type:

"https://github.com/user-attachments/assets/022dc199-35db-4999-a119-0917483ee9c4"


- Step 9 :Create new measure as Average Loan Amount by Age Groups (DAX Used : AVERAGE, AVERAGEX & VALUES)

DAX Function:


        Average Loan Amount by Age Group = 
        AVERAGEX(VALUES('Loan_default'[Age group]),
        AVERAGE('Loan_default'[LoanAmount]))

A Line chart was used to represent  Average Loan Amount by Age Groups:

"https://github.com/user-attachments/assets/6622ced4-8b9b-4dad-9403-8c29475efee4" 


- Step 10 : Create new measure as  Default Rate by Year (DAX Used :CALCULATE, COUNTROWS, ALLEXCEPT, FILTER, DIVIDE)

DAX Function:

        Default Rate by Year = 
        VAR totalloans=
               CALCULATE(COUNTROWS('Loan_default'),
               ALLEXCEPT('Loan_default',Loan_default[Year]))

        VAR default = CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),ALLEXCEPT('Loan_default',Loan_default[Year]))

        RETURN
        DIVIDE(default,totalloans)*100

A Line chart was used to represent Default Rate by Year:


"https://github.com/user-attachments/assets/2b2a2c34-4409-4771-83ee-51102fe36f61"


- Step 11 :  Median calculation & validation using MEDIANX DAX Function

- Step 12 :Create a  new measureas as Median Loan Amount by credit score category

DAX Function:

        Median by Credit score bins = 
        MEDIANX('Loan_default','Loan_default'[LoanAmount])

 A Line chart was used to represent Median Loan Amount by credit score category:

 "https://github.com/user-attachments/assets/763c0b0f-6e98-46df-97ec-8617e0cab07b


- Step 13 : Adding Donut Chart to show Average Loan Amount by Age Group & Marital Status

DAX Function :

        Average Loan Amt (High Credit) = 
        AVERAGEX(FILTER('Loan_default','Loan_default'[Credit Score Bins]="High"),'Loan_default'[LoanAmount])

https://github.com/user-attachments/assets/17948b7d-147d-42a6-954d-808bc45ee7e1

- Step 14 : Create a new measure as Loan(Adults) by credit categories (DAX Used : CALCULATE,AVERAGEX,SUM)
        
 DAX Function:

        Total Loan (Credit Bins) = 
        CALCULATE(SUM('Loan_default'[LoanAmount]),'Loan_default'[Age group]="Adults",ALLEXCEPT('Loan_default','Loan_default'[Age],Loan_default[Age group],'Loan_default'[CreditScore],'Loan_default'[Credit Score Bins]))

A Line chart was used to represent Loan(Adults) by credit categories:

https://github.com/user-attachments/assets/01788a41-6158-4ddf-9e6f-1bb68ec71a8a

- Step 15 : New measure was created to find Total Loan (Middle Age Adults) by have Mortgage/Dependents

DAX Function:

        Total Loan (Middle age adult) = 
        SUMX(FILTER('Loan_default','Loan_default'[Age group]="Middle Age Adult"),'Loan_default'[LoanAmount])

A clusterad column chart was used to representTotal Loan (Middle Age Adults) by have Mortgage/Dependents

https://github.com/user-attachments/assets/76961b48-1eaf-47c2-a009-3313801e80eb
        
 - Step 16 :New measure was created to find Loans by education type

 DAX Function:

         Loan by Education Type = 
        COUNTROWS(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanID]))))

A Line chart was used to represent Loans by education type:

"https://github.com/user-attachments/assets/32251094-0fc0-45d6-8626-aa2b01f108a7

- Step 17 :Creating the YOY Loan amount DAX Measure
DAX Function:

        YTD Loan Amount = 
        CALCULATE(SUM('Loan_default'[LoanAmount]),DATESYTD('Loan_default'[Loan Date (DD/MM/YYYY)]),ALLEXCEPT('Loan_default','Loan_default'[Credit Score Bins],Loan_default[MaritalStatus]))

A Line chart was used to represent YTD Loan Amount:

https://github.com/user-attachments/assets/c0303563-fab2-42b0-a587-beb2015a15aa

- Step 18 : Creating YOY Default Loans change DAX Measure

DAX Function: 

        YOY Default Loan Change = 
        DIVIDE(
    CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan Date (DD/MM/YYYY)])))

    ,CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan Date (DD/MM/YYYY)]))-1),0)*100

A Line chart was used to represent YOY Default Loan Change:

https://github.com/user-attachments/assets/67015de0-5fab-4dec-8de9-e774f02235db

- Step 19 : Adding Ribbon Chart to show Loan Amount by Credit Score Bins and MaritalStatus

https://github.com/user-attachments/assets/4dc894c1-eb1e-4e93-a924-5709e2f6fcbd

- Step 20 :Adding Decomposition Tree (DAX Used : SWITCH Function)

"https://github.com/user-attachments/assets/aea5b492-63d7-4fed-81e3-fd2da2f8b5fd"
















# Report Snapshot (Power BI DESKTOP)
Loan Default & Overview

https://github.com/user-attachments/assets/9e8473cc-5e1f-4584-8044-99940250db01

Applicant Demographics & Financial Profile
 # Report Snapshot (Power BI DESKTOP)

 
"https://github.com/user-attachments/assets/772dcb7b-2b47-42fc-b251-282f73c7bac1"

# Report Snapshot (Power BI DESKTOP)

Financial Risk Metrics

"https://github.com/user-attachments/assets/aa9e4eed-2f23-4e58-b15d-6ea57b02da13"



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













