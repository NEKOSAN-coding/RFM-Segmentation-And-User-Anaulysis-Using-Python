<img width="1755" height="541" alt="image" src="https://github.com/user-attachments/assets/eddd9a8d-02e7-4b17-bbcf-04fb795e7e1d" /><img width="965" height="598" alt="image" src="https://github.com/user-attachments/assets/88c66673-47d3-4b0c-93e6-463d8e71a20a" /># 🛒 Customer Segmentation For Marketing Campaigns in A Retail Global SuperStore | Python

**Author:** Hà Minh Khuê

**Date:** 05/08/2025

**Tools Used:** Python

## 📑 Table of Contents 

[📌 1. Background & Overview](#background--overview)  
[📂 2. Dataset Description & Data Structure](#dataset-description--data-structure)  
[🧹 3. Data Cleaning & Preprocessing](#data-cleaning--preprocessing)  
[🔍 4. Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
[🧮 5. Apply RFM Model](#apply-rfm-model)  
[📊 6. Visualization & Analysis](#visualization--analysis)  
[💡 7. Insight & Recommendation](#insight--recommendation)

## 1. 📌 Background & Overview

### Objective ###

📖 **What is this project about?**  

- SuperStore is a global retail company with a large customer base. For the upcoming Christmas and New Year campaigns, the Marketing Department aims to show appreciation to loyal customers and identify potential ones for targeted engagement.

- Given the growing data volume, manual segmentation using Excel is no longer practical. The Marketing Director proposed using the **RFM (Recency, Frequency, Monetary)** model and has requested the Data Analytics Team to implement an automated **segmentation pipeline using Python** for **calculation, segmentation and visualization** to support data-driven marketing strategies..  

👤 **Who is this project for?**  

✔️ Marketing & Sales Department  
✔️ Stakeholders & Direct Manager

❓ **Business Questions:**  

✔️ How can we segment customers effectively using the RFM model?  
✔️ Which customer groups should be prioritized for retention and promotional campaigns?  
✔️ What actionable insights can help improve marketing strategies and customer engagement?  
✔️ What strategies should be applied to different customer segments to maximize value?  

### RFM Analysis Overview  

**🔎 Why use RFM?**  

RFM (Recency, Frequency, Monetary) is a customer analysis technique based on purchasing behavior. In RFM analysis, each customer is assigned a score based on these three factors:
- **Recency**: Measures the time elapsed since a customer's last purchase.  
- **Frequency**: Evaluates how often a customer makes transactions.  
- **Monetary**: Calculates the total amount of money spent by the customer.
The data is then used to categorize customers into segments, helping businesses identify key customers for targeted marketing and sales strategies. By applying RFM, businesses can segment customers based on their value, allowing them to optimize marketing and customer engagement strategies.  

## 2. 📂 Dataset Description & Data Structure

### 📌 Data Source  
- **Source**: Provided dataset for E-commerce retail analysis  
- **Size**: 541,910 rows × 8 columns (Sheet 1: E-commerce retail), additional segmentation details in Sheet 2  
- **Format**: .xlsx (Excel file with two sheets)  
## 📂 Data Structure & Relationships  

### 1️⃣ Tables Used  
The dataset consists of **two tables (sheets)**:  
- **Sheet 1: E-commerce Retail** – Contains transaction-level data, including order details, customer IDs, and purchase information.  
- **Sheet 2: Segmentation** – Stores customer segments along with their RFM scores.  

### 2️⃣ Table Schema & Data Snapshot  

#### 📌 Sheet 1: E-commerce Retail  
### 📋 Table Schema: E-commerce Retail  

<details>
  <summary>📂 **Dataset Schema** (Click to expand)</summary>

| Column Name  | Data Type         | Description  |  
|-------------|-----------------|--------------|  
| **InvoiceNo**  | `object`  | Unique invoice number for each transaction (6-digit). If it starts with 'C', it indicates a cancellation. |  
| **StockCode**  | `object`  | Unique product (item) code (5-digit). |  
| **Description**  | `object`  | Product (item) name. |  
| **Quantity**  | `int64`  | The number of units purchased per transaction. |  
| **InvoiceDate**  | `datetime64[ns]`  | Date and time when the transaction occurred. |  
| **UnitPrice**  | `float64`  | Price per unit of the product in sterling. |  
| **CustomerID**  | `float64`  | Unique 5-digit identifier for each customer. |  
| **Country**  | `object`  | Name of the country where the customer resides. |  

</details>

#### 📌 Sheet 2: Segmentation  
### 📊 Customer Segmentation & RFM Scores  

<details>
  <summary>📊 **RFM Segmentation Mapping** (Click to expand)</summary>

| **Segment**              | **RFM Score**  |  
|-------------------------|-----------------------------------------------------------|  
| **Champions**            | 555, 554, 544, 545, 454, 455, 445  |  
| **Loyal**                | 543, 444, 435, 355, 354, 345, 344, 335  |  
| **Potential Loyalist**   | 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323  |  
| **New Customers**        | 512, 511, 422, 421, 412, 411, 311  |  
| **Promising**            | 525, 524, 523, 522, 521, 515, 514, 513, 425, 424, 413, 414, 415, 315, 314, 313  |  
| **Need Attention**       | 535, 534, 443, 434, 343, 334, 325, 324  |  
| **About To Sleep**       | 331, 321, 312, 221, 213, 231, 241, 251  |  
| **At Risk**              | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124  |  
| **Cannot Lose Them**     | 155, 154, 144, 214, 215, 115, 114, 113  |  
| **Hibernating Customers** | 332, 322, 233, 232, 223, 222, 132, 123, 122, 212, 211  |  
| **Lost Customers**       | 111, 112, 121, 131, 141, 151   |

</details>

## 3. 🧹 Data Cleaning & Preprocessing

[In 1]:  
```python
# Print the first five rows of the dataset
ssdata.head()
# Detect data values
ssdata.describe()
# Information dataset
ssdata.info()
```
[Out 1]:

<img width="941" height="756" alt="image" src="https://github.com/user-attachments/assets/9d8de8ee-c365-4bd4-b339-39bb4acc6691" />

#### Summary:

⚡During the data exploration, The **Quantity** and **Unit Price** columns contain negative values. This is not logically valid and requires further investigation. Actions need to be done:
- Checking for data entry errors.
- Identifying if there are negative values indicate refunds or cancellations.
- Removing or adjusting incorrect data to ensure accuracy.

#### ⚡ Data Quality Insights

During the data exploration, an error was founded between **Stock Code** count (3958) and **Description** count (4207) using Excel fomular: '=COUNTA(UNIQUE())'. This may lead to some deeper investigation. Possible reasons the data should had:

- Stock code of some products may share multiple descriptions.
- Some descriptions might not be linked to any valid stock code.
- Data inconsistencies due to missing, duplicated, or improperly recorded.

### 🔎 Data Validation  
- **Checked Negative Quantities & Cancellations**: Identified if negative **Quantity** values correspond to cancellations by verifying **InvoiceNo** starting with "C".  
-> Review cancellations to ensure data integrity before analysis.  

#### 🚨 Identifying Invalid Transactions

After flagging cancellations, verify if there are any transactions where **Quantity** is negative, but the **InvoiceNo** does not start with "C". These may indicate data inconsistencies.

### ✨ Conclusion:

**Data Types:**
The following columns have inappropriate data types and should be converted to **strings** for easier processing:
- **InvoiceNo**, **StockCode**, **Description**, **CustomerID**, **Country**.

**Data Values:**
- **Quantity < 0 & InvoiceNo starts with 'C'** → These transactions indicate **canceled orders** and should be **removed** from the dataset.
- **Quantity < 0 but InvoiceNo does NOT start with 'C'** → These records contain **incorrect descriptions** and should be **excluded** from the dataset.
- **UnitPrice < 0 & incorrect Description** → These are **invalid transactions** and should also be **removed** from the dataset.

## 4. 🔍 Exploratory Data Analysis (EDA)

### 🛠 Step 1. Deepdive into dataset

[In 2]:
```python
# Using ProfileReport for further exploratory
profile = ProfileReport(ssdata)
profile
```
[Out 2]:

<img width="1744" height="781" alt="image" src="https://github.com/user-attachments/assets/70ad6d11-2266-4b47-9019-53cd9c75d315" />

### 🛠 Step 2. Removing Invalid Transactions (Quantity < 0; Price < 0)


[In 3]:

```python
# Handle unreasonable data types and data values
# Convert specific fields to the correct format
print(ssdata.columns)
print('')


column_list = ['InvoiceNo','StockCode','Description','CustomerID','Country']
for c in column_list:
     ssdata[c] = ssdata[c].astype(str)

# Drop inappropriate data values
## Drop data where UnitPrice < 0
ssdata = ssdata[ssdata['UnitPrice'] > 0]
## Drop data where Quantity < 0
ssdata = ssdata[ssdata['Quantity'] > 0]
## Drop canceled transactions
ssdata = ssdata[ssdata.check_cancel == False]
ssdata = ssdata.replace('nan', None)
ssdata = ssdata.replace('Nan',None)
ssdata.shape
```
[Out 3]:

<img width="538" height="86" alt="image" src="https://github.com/user-attachments/assets/63c66fce-52fc-4df1-aa14-b9fa88e64de8" />


### 🛠 Step 3. 🛠 Checking and Removing Missing Values in CustomerID and Descriptions

Since CustomerID is essential for valid transactions, records lacking it should be removed for consistency.

[In 4]:

```python
# @title Handle missing value
ssdata = ssdata[ssdata['CustomerID'].notnull()]
ssdata = ssdata[ssdata['Descriptions'].notnull()]
ssdata.head()```

[Out 4]:

<img width="1169" height="196" alt="image" src="https://github.com/user-attachments/assets/af81f83b-5990-46bf-a8e9-486fa96ef4c0" />


#### 🔍 Investigating Missing CustomerID and Descriptions:

Before executing the code: It is important to check if the missing **CustomerID** and **Descriptions** is concentrated in specific countries or time periods before deciding how to handle it.

**⁉️ Findings:**
- Mostly missing **CustomerID** and **Description** are lacking of record in **United Kingdom** and in a long time series. It's likely due to **error recorded system** or **human false**

**✅ Solution:**
Since **CustomerID is essential**, drop missing values to maintain data integrity.
Lacking of **Description** are likely due to an **error recorded**, also drop missing values as well.

### 🛠 Step 4. Handle duplicate**

[In 5]: 

```python
ssdata_duplication = ssdata.duplicated(subset=["InvoiceNo", "StockCode","InvoiceDate","CustomerID"])
print (ssdata[ssdata_duplication].shape)
print ('')
print (ssdata.shape)
```

[Out 5]:

<img width="1169" height="196" alt="image" src="https://github.com/user-attachments/assets/60b82b40-6684-4106-9494-4efb737a13cc" />


The result (10038, 11) means there are 10,038 duplicate rows, and they need to be detected and handled
## 5. 🧮 Apply RFM Model

#### 🛠 Step 1. Calculate RFM Score

[In 6]:

```python
#Calculate the last transaction date  
last_day = ssdata_drop_duplications['Day'].max()

# Create revenue column 
ssdata_drop_duplications['Revenue'] = ssdata_drop_duplications['Quantity'] *  ssdata_drop_duplications['UnitPrice']  

# Create RFM 
RFM_ssdata = ssdata_drop_duplications.groupby('CustomerID').agg(
    Recency = ('Day', lambda x: last_day - x.max()),
    Frequency = ('CustomerID','count'),
    Monetary = ('Revenue','sum'),
    Start_Day = ('Day', 'min')).reset_index()

RFM_ssdata['Recency'] = RFM_ssdata['Recency'].dt.days.astype('int16')
RFM_ssdata['Recency_reverse'] = - RFM_ssdata['Recency'] 
RFM_ssdata['Start_Day'] = pd.to_datetime(RFM_ssdata['Start_Day'])
RFM_ssdata['Start_Month'] = RFM_ssdata['Start_Day'].apply(lambda x : x.replace(day=1))
```

#### 🛠 Step 2. Assign RFM scores using Qcut

[In 7]:

```python
# using qcut to create R, F, M
RFM_ssdata['R'] = pd.qcut(RFM_ssdata["Recency_reverse"], 5, labels = range(1, 6)).astype(str) # Score recency
RFM_ssdata['F'] = pd.qcut(RFM_ssdata["Frequency"], 5, labels = range(1, 6)).astype(str)
RFM_ssdata['M'] = pd.qcut(RFM_ssdata["Monetary"], 5, labels = range(1, 6)).astype(str)
RFM_ssdata['RFM'] = RFM_ssdata.apply(lambda x: x.R + x.F + x.M, axis = 1)
RFM_ssdata.head()```

[Out 7]:

<img width="743" height="167" alt="image" src="https://github.com/user-attachments/assets/15677fda-e0e2-48d2-a8a4-79b0a5ab7449" />


#### 🛠 Step 4. Calculate RFM Score, merge into dataset

Process the segmentation table & merge with RFM_df

[In 8]:

```python
# segmentation dataset
seg = pd.read_excel(path + 'ecommerce_retail.xlsx', sheet_name = 'Segmentation')
seg['RFM Score'] = seg['RFM Score'].str.split(',')
seg = seg.explode('RFM Score').reset_index(drop = True)

# clean FRM dataset
seg['RFM Score'] = seg['RFM Score'].apply(lambda x: x.strip())

# merge proper Segmentation
RFM_ssdata_final = RFM_ssdata.merge(seg, how = 'left', left_on = 'RFM', right_on = 'RFM Score')
RFM_ssdata_final.head()

```
[Out 8]:

<img width="945" height="181" alt="image" src="https://github.com/user-attachments/assets/0486a172-a6c9-4ad7-925e-33e8faaa815c" />


## 6. 📊 Visualization & Analysis

# 1. Number of Customers per Segmentations

<img width="943" height="444" alt="image" src="https://github.com/user-attachments/assets/342a51c9-fb10-4236-9382-842cb3061728" />

# 2. Recency distribution by Segmentations

<img width="965" height="598" alt="image" src="https://github.com/user-attachments/assets/8b51db5e-03b1-4c93-abe6-206a2ab4dbde" />

# 3. Relationships among RFM via Heatmap

<img width="1755" height="541" alt="image" src="https://github.com/user-attachments/assets/56265838-51b5-437e-89cf-2076dcf95b3e" />

# 4. Histogram of R,F,M distributions

<img width="1669" height="445" alt="image" src="https://github.com/user-attachments/assets/11e2486e-6101-48fe-9307-5d42be871f78" />

# 5. Revenue by Segmentations

<img width="1769" height="324" alt="image" src="https://github.com/user-attachments/assets/8e5559a3-feda-428e-876b-448d8fa21c21" />

--- 

### 📊 Observations 

# RFM Analysis — Storytelling by Each Chart

## 1) Bar Chart — Customer Share by Segment

Looking at the bar chart, the customer segmentation picture is quite clear:

**Champions:** With **783 customers (~18%)**, this group is not only large but also contributes **over 60% of total revenue**. They’ve purchased recently, purchase frequently, and spend a lot — true VIPs. This is the revenue engine and should be retained with special appreciation programs, exclusive perks, early access to new products, and cross-sell/upsell nudges.

**Hibernating customers:** The **largest group (816 customers, ~19%)**, but only **3.7% of revenue**. They purchased in the past but are now “asleep,” contributing almost no revenue. If ignored, they will slip into “Lost.” This is a recoverable pool that needs strong reactivation.

**Loyal:** **423 customers (~9.75%)**, contributing **11.7% of revenue**. They buy steadily but with modest order value — there’s room to lift revenue per customer via product bundles, memberships, or upgrade incentives.

**At Risk:** **427 customers (~9.84%)**, formerly high-value but drifting away. Without early intervention, this group will drive significant revenue loss.

**Need Attention:** **225 customers (~5%)**. Though small, they’re the “tipping point” between loyal and at-risk. If overlooked, they can quickly slide into “At Risk” or “Hibernating.”

**Overall:** Two clear extremes: **Champions** concentrate value, while **Hibernating** and **At Risk** concentrate missed revenue.

---

## 2) Boxplot — Recency by Segment

The Recency boxplot reveals how recently each group purchased:

**Champions:** Clustered at **low Recency** (very recent purchases) — a very positive signal of active engagement. A few outliers exist but are negligible.

**Loyal:** **Lower-than-average Recency**, showing steady purchasing, though less frequent than Champions.

**Potential Loyalist & Need Attention:** **Mid-range Recency** with wide spread — they still engage but are easy to lose without a nudge.

**At Risk, Hibernating, Cannot Lose Them:** **Very high Recency** — it’s been a long time since their last purchase. These groups need urgent campaigns to bring them back.

**Key insight:** The Recency gap between Champions and the risk groups is **very large**, indicating polarized behavior — either highly engaged or nearly churned.

---

## 3) Heatmap — RFM Relationships

**R vs F (Recency vs Frequency):**  
Top-right (high R, high F) concentrates VIPs like **Champions** and **Loyal**.  
Bottom-left (low R, low F) is the majority — customers who purchased long ago and very few times, mainly **Hibernating/Lost**.

**F vs M (Frequency vs Monetary):**  
A clear **positive relationship**: customers who buy more often tend to spend more. This supports upsell focus on **Loyal/Potential Loyalist**.  
There’s a small group with **low F but high M** — few purchases but large basket size. These are **potential VIPs** deserving personalized care.

**R vs M (Recency vs Monetary):**  
**High R – High M** are **current VIPs (Champions)**.  
**Low R – High M** are **formerly high spenders** who haven’t returned for a while — **top reactivation targets**.

---

## 4) Histograms — Distributions of Recency, Frequency, Monetary

**Recency:**  
Right-skewed; most customers purchased within the **last 20–50 days**.  
There’s still a group with **>300 days** since last purchase — **At Risk/Hibernating/Lost**.

**Frequency:**  
Highly right-skewed; most customers purchase **1–5 times**.  
Customers with **>30 purchases** are extremely rare but very valuable.

**Monetary:**  
Large disparity; most customers spend **under 5,000**.  
A tiny **very-high-spend** group contributes a big share of revenue.

**Takeaway:** Revenue depends heavily on a **very small VIP cohort**, while the majority buy infrequently and spend little — a risk if VIPs churn.

---

## 5) Treemap — Revenue by Segment

**Champions** dominate the treemap area → revenue is heavily concentrated in this VIP group.  
**Loyal** and **At Risk** form a second tier but still far behind Champions — losing even a small portion of Champions would cause a severe revenue drop that’s hard to offset.  
**Hibernating** has **high customer count (~19%)** yet appears tiny on the treemap → **very low revenue** — a major untapped opportunity.  
**Need Attention** and **Potential Loyalist** are small — they haven’t created significant revenue yet despite remaining active. They need activation strategies to purchase more.

**Key takeaways from the treemap:**  
- Revenue is **highly concentrated** → diversify by lifting value from **Loyal** and **Potential Loyalist**.  
- Large-in-count but low-value groups like **Hibernating** can fuel growth with an effective **reactivation** play.  
- Use the treemap to **prioritize marketing** toward segments with large area and fast growth potential.

---

## 6) Conclusions & Strategies

**Retain VIPs (Champions, Loyal)**  
- VIP club, birthday perks, early access.  
- Cross-sell/upsell to premium products.

**Convert potential segments (Potential Loyalist, Need Attention)**  
- Behavior-based remarketing.  
- Special incentives to repurchase within **30 days**.

**Reactivate risk segments (At Risk, Hibernating)**  
- FOMO campaigns, limited-time vouchers.  
- Exit surveys on reasons for churn, surface new product suggestions.

**Optimize marketing budget**  
- Use the heatmap to focus on **High R – High M** and **Low R – High M** cohorts.



