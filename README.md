# Exploratory-Data-Analysis-for-Ed-tech-Users

import pandas as pd
#Importing pandas library

df = pd.read_excel(r"assignment_sample_data.xlsx")
#reading the data from the excel file

df.head()
#Displaying first 5 rows of the data 

df.shape
#To obtain the shape of the dataframe(no of rows & column)

df.info()
#getting the summary of the dataframe

df.tail()
#Analyzing last 5 rows of the data

df = df.drop(2000)
#removing the last row from the date for the data cleaning 

#From the sample data provided, make a week wise report of win back users. Users who joined back in 7- 60 days after churning are called win back users. Also, get any possible insights that you can make from the data.(use python to complete this task)

df = df.sort_values(by = ['account_number','invoice_date','payment_date'],ascending = [True, True, True],na_position = 'first')
df
#Organising & sorting the data 

df[df['account_number'] == '01e0cf-55f8-f892']
#checking records for a random account

df[df['account_number'] == '086b2e-695e-5e53']['invoice_date'].shift()

df['prior_invoice_date'] = df.groupby(['account_number'])['invoice_date'].shift()
df.head()
#creating a new column to fetch the previous invoice date

df['prior_payment_status'] = df.groupby(['account_number'])['payment_status'].shift()
df
#creating a new column to fetch the previous payment status

df[df['account_number'] == '086b2e-695e-5e53']

df['difference'] = df['payment_date'] - df['prior_invoice_date']
#calculating the difference between payment date and prior invoice date

df.info()

win_back_users = df[(df['payment_status'] == 'Processed') & 
                        (df['prior_payment_status'] == 'Error') & 
                        (df['difference'] >= pd.Timedelta(days=7)) & 
                        (df['difference'] <= pd.Timedelta(days=60))]
#calculating win_back_users with the given condition

week_wise_users = win_back_users.groupby(pd.Grouper(key='payment_date', freq='W-MON'))['account_number'].nunique()
week_wise_users
#calculating win_back_users per week


