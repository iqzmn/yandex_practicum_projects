### **Project Goal**  
When analyzing bank clients' creditworthiness statistics, the question arises about the impact of marital status and the number of children on timely loan repayment. Answering this question is the goal of this project and holds great significance for the banking sector, as it helps better understand the risks associated with issuing loans. Another crucial aspect is the overall financial status of the family, income level, and financial discipline of the client. Banks and other lenders must consider all these factors when making loan decisions to minimize risks and ensure their financial stability.  

The findings of this study will be used to develop a **credit scoring model**. Credit scoring is an important tool for financial institutions in deciding loan approvals. It is based on statistical analysis of various factors, such as **payment history, income level, age, education, and other variables** that may influence a borrower’s ability to repay the loan on time.  

Machine learning methods will be used to develop the credit scoring model. The target variable is an indicator that shows whether the client had past loan defaults or not.  

### **Project Objectives**  
During the project, we will answer the following questions:  
- Is there a relationship between the number of children and on-time loan repayment?  
- Is there a relationship between marital status and on-time loan repayment?  
- Is there a relationship between income level and on-time loan repayment?  
- How do different loan purposes affect repayment on time?  

### **Client Creditworthiness Data Includes the Following Criteria:**  
- `children` — number of children in the family  
- `days_employed` — total work experience in days  
- `dob_years` — client’s age in years  
- `education` — client’s education level  
- `education_id` — education level identifier  
- `family_status` — marital status  
- `family_status_id` — marital status identifier  
- `gender` — client’s gender  
- `income_type` — type of employment  
- `debt` — whether the client had past loan defaults  
- `total_income` — monthly income  
- `purpose` — purpose of obtaining the loan  

### **Main Project Stages:**  
1. **Data Loading**  
2. **Exploratory Data Analysis**  
3. **Research Questions**  
4. **Machine Learning Model Development**  
5. **Final Conclusions**  

### **Summary of Data Processing**  
The provided data was in text format and required preprocessing for further analysis. During the exploratory data analysis, the following actions were taken:  
- Identified and removed **both explicit and implicit duplicates**  
- **Handled missing values**  
- Changed **data types** for some features  
- **Corrected negative values**  
- **Removed outliers**  
- **Categorized some features**  

### **Key Insights from Data Analysis**  
- **Clients with secondary education** take loans most frequently, followed by those with higher education. Clients with unfinished higher education, primary education, or an academic degree take loans less frequently.  
- **Married individuals** apply for loans most often. **Unmarried** individuals and those in a **civil partnership** apply for loans less frequently. **Divorced and single individuals** take loans the least often.  
- **Women apply for loans more often than men.**  
- **Employees and entrepreneurs** take loans more often than **retirees.** Government employees apply for loans the least frequently.  
- The most **popular loan purpose** is **real estate transactions**, followed by **car purchases** and **education expenses**. The least common loan purpose is **wedding expenses**.  
- Clients **without children** apply for loans most frequently, followed by those with **one, two, and three children** in descending order.  
- The **average borrower age is 39 years**.  
- **8% of borrowers had difficulty repaying their loans.**  
- The majority of loan applicants earn between **50,000 and 200,000**, representing **middle-income** earners. The second largest group consists of individuals with **above-average income**.  
- As **work experience increases, the likelihood of taking a loan decreases**. Clients with less than **10 years of experience** are the most frequent borrowers.  

### **Key Findings on Creditworthiness**  
- Borrowers with **three children and those without children** have the highest creditworthiness. **Clients with two children are more creditworthy than those with one child**.  
- **Single individuals with prior marriage experience** repay debts more easily than **currently married individuals**. However, those who **have never been married** struggle the most with repayments.  
- Borrowers with a **middle income level** struggle the most with debt repayment. Next are **high-income and very high-income clients**. Borrowers with **below-average income** tend to repay debts more easily.  
- **Car purchases and education expenses** have **the highest loan default rates**, while **real estate transactions and wedding expenses** have the **lowest default rates**.  

### **Machine Learning Implementation**  
During machine learning model development, the class imbalance of the target variable was addressed using **RandomOverSampler()**.  

Key findings:  
- **Income level, work experience, and age** are the most important factors when deciding on loan approvals.  
- A **decision tree** was used as a **binary classification algorithm** for training the model.  
- The following metrics were used to evaluate model performance:  
  - **Accuracy**  
  - **Precision**  
  - **Recall**  
  - **F1-score**  
  - **Confusion matrix**  
  - **AUC (Area Under the Curve) = 0.95**  

The results indicate that **income level, work experience, and age significantly influence credit approval decisions**. The developed model provides a solid foundation for optimizing **credit scoring** and minimizing financial risks for lending institutions.
