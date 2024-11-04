### RFM Analysis: Overview
RFM Analysis is a concept used by Data Science professionals, especially in the marketing domain for understanding and segmenting customers based on their buying behaviour.

Using RFM Analysis, a business can assess customers’:

recency (the date they made their last purchase)

frequency (how often they make purchases)

and monetary value (the amount spent on purchases)

Recency, Frequency, and Monetary value of a customer are three key metrics that provide information about customer engagement, loyalty, and value to a business.

### RFM Analysis using Python
I’ll start the task of RFM Analysis by importing the necessary Python libraries.
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
```

### Calculating RFM Values
I’ll now calculate the Recency, Frequency, and Monetary values of the customers to move further:
```python
df['recency'] = (datetime.now().date() - df['PurchaseDate'].dt.date).dt.days
frequency_df=df.groupby(['CustomerID'])['OrderID'].count().reset_index()
frequency_df.rename(columns={'OrderID':'frequency'},inplace=True)
df=df.merge(frequency_df,on='CustomerID',how='left')
monetary_df=df.groupby(['CustomerID'])['TransactionAmount'].sum().reset_index()
monetary_df.rename(columns={'TransactionAmount':'monetary'},inplace=True)
df=df.merge(monetary_df,on='CustomerID',how='left')
```

### Calculating RFM Scores:
Now let’s calculate the recency, frequency, and monetary scores:
```python
recency_segemnt=[5,4,3,2,1]
frequency_segment=[1,2,3,4,5]
monetary_segment=[1,2,3,4,5]

df['recency_score']=pd.cut(df['recency'],bins=5,labels=recency_segemnt)
df['frequency_score']=pd.cut(df['frequency'],bins=5,labels=frequency_segment)
df['monetary_score']=pd.cut(df['monetary'],bins=5,labels=monetary_segment)

df['recency_score']=df['recency_score'].astype(int)
df['frequency_score']=df['frequency_score'].astype(int)
df['monetary_score']=df['monetary_score'].astype(int)

df['rfm_score']=df['recency_score']+df['frequency_score']+df['monetary_score']
```

### RFM Value Segmentation:

Now let’s calculate the final RFM score and the value segment according to the scores:
```python
value_segment=['Low_Value','Mid_Value','High_Value']
df['value_segment']=pd.qcut(df['rfm_score'],q=3,labels=value_segment)
```

### RFM Customer Segments:
The above segments that we calculated are RFM value segments. Now we’ll calculate RFM customer segments. The RFM value segment represents the categorization of customers based on their RFM scores into groups such as “low value”, “medium value”, and “high value”. These segments are determined by dividing RFM scores into distinct ranges or groups, allowing for a more granular analysis of overall customer RFM characteristics. The RFM value segment helps us understand the relative value of customers in terms of recency, frequency, and monetary aspects.

Now let’s create and analyze RFM Customer Segments that are broader classifications based on the RFM scores. These segments, such as “Champions”, “Potential Loyalists”, and “Can’t Lose” provide a more strategic perspective on customer behaviour and characteristics in terms of recency, frequency, and monetary aspects. Here’s how to create the RFM customer segments:

```python
def rfm_seg(value):
    if value >= 9:
        return "Champion"
    elif value >= 6 and value < 9:
        return 'Potential Loyalists'
    elif value >= 5 and value < 6:
        return 'At Risk Customers'
    elif value >= 3 and value < 5:
        return 'Lost'
    else:
        return 'Other'

df['Customer_Segment']=df['rfm_score'].apply(rfm_seg)

df['Customer_Segment'].value_counts()
```
### Dashboard Development using Power BI:
Integrate Power BI with Python Data. Then I created Dashboar.

#### OverView Dashboard:

![p_1](https://github.com/user-attachments/assets/02d6e27f-885f-435f-a86f-eddaab66b575)

#### Customer Dashboard:
![p_2](https://github.com/user-attachments/assets/04d63c7f-92c1-4b37-b363-eefc8fa8d140)

### Summary
RFM Analysis is used to understand and segment customers based on their buying behaviour. RFM stands for recency, frequency, and monetary value, which are three key metrics that provide information about customer engagement, loyalty, and value to a business. I hope you liked this article on RFM Analysis using Python. Feel free to ask valuable questions in the comments section below.
