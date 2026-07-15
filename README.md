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
