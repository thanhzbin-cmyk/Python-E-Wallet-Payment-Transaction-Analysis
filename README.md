# Python-E-Wallet-Payment-Transaction-Analysis
This project analyzes payment and transaction data from an e-wallet platform using Python and Pandas. The analysis focuses on data quality assessment, payment performance, product contribution, refund behavior, and transaction classification to generate business insights and support operational decision-making.

## Dataset ##
| Dataset                | Description                                                                          |
| ---------------------- | ------------------------------------------------------------------------------------ |
| **payment_report.csv** | Monthly payment volume by product                                                    |
| **product.csv**        | Product information including category and team ownership                            |
| **transactions.csv**   | Transaction records with sender, receiver, merchant, source, and transaction details |

## Business Questions ##

### Part I – Exploratory Data Analysis ###
- Assess data quality for each dataset
- Detect missing values, duplicates, incorrect data types, and invalid values
- Summarize numerical variables
- Prepare clean datasets for analysis
### Part II – Business Analysis ###
**Using payment_report and product**
- Identify the Top 3 products with the highest payment volume
- Detect products assigned to multiple teams
- Evaluate team performance since Q2 2023
- Identify the weakest-performing category within the lowest-performing team
- Analyze refund transaction sources and their contribution

**Using transactions**
- Classify transaction types based on business rules
- Analyze transaction count
- Analyze transaction volume
- Analyze unique senders
- Analyze unique receivers for each transaction type
--------------
# Exploratory Data Analysis #

## 1. Data Preparation ##
- Merge payment records with product information to create a unified dataset for subsequent analysis.
<img width="1374" height="72" alt="image" src="https://github.com/user-attachments/assets/37c2e529-2d91-49bc-8792-3beef5d99ac5" />

- Dataset preview
  
**Table: dfpayment_enriched**
<img width="1332" height="342" alt="image" src="https://github.com/user-attachments/assets/ee1badca-f90b-4219-bf15-f0955d30f173" />

**Table: dftransactions**
<img width="1224" height="312" alt="image" src="https://github.com/user-attachments/assets/1124702b-7988-4a2e-9b1e-6c738c259f09" />

## 2. Data Quality Assessment ##

### 2.1 Structure Overview ###

**Table: dfpayment_enriched**

- dfpayment_enriched.info()

| Check          | Result                                       | Action                             |
| -------------- | -------------------------------------------- | ---------------------------------- |
| Missing values | 22 missing rows in `category` and `team_own` | Investigate unmatched `product_id` |
| Data type      | `report_month` stored as object              | Convert to datetime                |
| Columns        | All expected columns available               | No action                          |

**Table: dftransactions**
- dftransactions.info()

| Check          | Result                                                              | Action                                                                                    |
| -------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Missing values | `sender_id`, `receiver_id`, and `extra_info` contain missing values | Investigate whether the missing values are expected business cases or data quality issues |
| Data type      | `timeStamp` stored as Unix timestamp (`int64`)                      | Convert to `datetime`                                                                     |
| Columns        | All expected columns available                                      | No action                                                                                 |

### 2.2 Numerical Variables ###

**Table: dfpayment_enriched**

<img width="1158" height="432" alt="image" src="https://github.com/user-attachments/assets/580c7fef-8704-4920-a1bf-0fd2312b7b25" />

- No missing in volume (count = 919)
- No obvious incorrect values (min > 0)
- Potential outliers: max much larger than Q3
- Distribution appears right-skewed (mean > median)
- High variability based on std

**Table: dftransactions**

<img width="1276" height="432" alt="image" src="https://github.com/user-attachments/assets/304e277c-9941-447c-8237-266ea3f49770" />

- Incorrect: receiver_id column has a negative value
- volume column: Potential outlier
- volume column: Distribution appears right-skewed (mean > median)
- volume column: High variability based on std
- volume column: data spread out

### 2.3 Categorical Variables ###

**Table: dfpayment_enriched**
dftransactions['transType'].value_counts()
dftransactions['transStatus'].value_counts()
dftransactions['sender_id'].value_counts()
dftransactions['receiver_id'].value_counts()

**Table: dftransactions**
dftransactions['transType'].value_counts()
dftransactions['transStatus'].value_counts()
dftransactions['sender_id'].value_counts()
dftransactions['receiver_id'].value_counts()

### 2.4 Distribution Analysis ###

**Table: dfpayment_enriched**
<img width="2560" height="728" alt="image" src="https://github.com/user-attachments/assets/cf25dac0-7f7c-496a-bb53-5a956e211b34" />

**Table: dftransactions**
<img width="2558" height="724" alt="image" src="https://github.com/user-attachments/assets/c9912aa5-89f9-4f57-8598-e5297b8a93f7" />

### 2.5 Data Validation ###

**Table: dfpayment_enriched**

- All payment_group values are valid (payment or refund).
- No duplicate records were detected.

**Table: dftransactions**

- Transaction-related fields contain valid values based on the business rules.
- No duplicate records were detected.

## 3. Data Summarization ##

**Table: dfpayment_enriched**
<img width="2560" height="1022" alt="image" src="https://github.com/user-attachments/assets/d4035bcc-3718-4ddc-8a30-73e48cf2ba34" />

**Table: dftransactions**

--------------
# Business Analysis #
- Q1: Top 3 product_ids with the highest volume.
- Q2.Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?
- Q3.Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.
- Q4. Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?
- Q5. Define type of transactions (‘transaction_type’) for each row, given:

transType = 2 & merchant_id = 1205: Bank Transfer Transaction |
transType = 2 & merchant_id = 2260: Withdraw Money Transaction |
transType = 2 & merchant_id = 2270: Top Up Money Transaction |
transType = 2 & others merchant_id: Payment Transaction |
transType = 8, merchant_id = 2250: Transfer Money Transaction |
transType = 8 & others merchant_id: Split Bill Transaction |
Remaining cases are invalid transactions

- Q6.Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.
