# 📊 Power BI Campaign Performance Analysis

## 🚀 Project Overview
Analyzed global social media ad campaign performance using Power BI across 4 datasets:
- users.csv
- ads.csv
- ad_events_sample.csv
- campaigns.csv

---

# 🔗 Data Model (Star Schema)

## ✅ Relationships

| From Table | Column | To Table | Column |
|-----------|--------|----------|--------|
| users     | user_id | ad_events | user_id |
| ads       | ad_id   | ad_events | ad_id |
| campaigns | campaign_id | ads | campaign_id |

✔ Relationship Type: Many-to-One  
✔ Cross Filter: Single Direction  

---

# 🧠 Calculated Columns

## 1️⃣ Event Classification
```DAX
Event Category =
SWITCH(
    TRUE(),
    ad_events[eventtype] = "impression", "Impression",
    ad_events[eventtype] = "click", "Click",
    ad_events[eventtype] = "engagement", "Engagement",
    ad_events[eventtype] = "purchase", "Conversion",
    "Other"
)
```
## ⏱️ Time-Based Columns

### 2️⃣ Time of Day Bucket
```DAX
TimeOfDay =
SWITCH(
    TRUE(),
    HOUR(ad_events[timestamp]) >= 6 && HOUR(ad_events[timestamp]) < 12, "Morning",
    HOUR(ad_events[timestamp]) >= 12 && HOUR(ad_events[timestamp]) < 17, "Afternoon",
    HOUR(ad_events[timestamp]) >= 17 && HOUR(ad_events[timestamp]) < 21, "Evening",
    "Night"
)
```
## ⏱️ Calculated Columns

### 3️⃣ Day of Week
```DAX
DayOfWeek =
FORMAT(ad_events[timestamp], "dddd")
```
## 👥 Audience Segmentation Column

### 4️⃣ Age Group (Users Table)
```DAX
Age Group =
SWITCH(
    TRUE(),
    users[age] < 18, "Under 18",
    users[age] <= 25, "18-25",
    users[age] <= 35, "26-35",
    users[age] <= 45, "36-45",
    "45+"
)
```
## 📐 Measures (DAX)

### 🔹 Core Metrics
```DAX
Total Impressions =
CALCULATE(
    COUNTROWS(ad_events),
    ad_events[Event Category] = "Impression"
)

Total Clicks =
CALCULATE(
    COUNTROWS(ad_events),
    ad_events[Event Category] = "Click"
)

Total Engagements =
CALCULATE(
    COUNTROWS(ad_events),
    ad_events[Event Category] = "Engagement"
)

Total Conversions =
CALCULATE(
    COUNTROWS(ad_events),
    ad_events[Event Category] = "Conversion"
)
```
## 📊 Performance Metrics

```DAX
CTR =
DIVIDE([Total Clicks], [Total Impressions], 0)

Conversion Rate =
DIVIDE([Total Conversions], [Total Clicks], 0)

Engagement Rate =
DIVIDE([Total Engagements], [Total Impressions], 0)
```
## 📊 Visualizations

### 1️⃣ Users by Day of Week (Bar Chart)
- **Axis:** DayOfWeek  
- **Values:** DISTINCTCOUNT(users[user_id])  

---

### 2️⃣ Users by Time of Day (Pie Chart)
- **Legend:** TimeOfDay  
- **Values:** DISTINCTCOUNT(users[user_id])  

---

## ⏱️ Time-Based Analysis

### 📈 Visuals:
- Impressions by DayOfWeek  
- Clicks by DayOfWeek  
- CTR by TimeOfDay  

---

### 🧠 DAX for CTR by Time
```DAX
CTR Time =
CALCULATE([CTR])
```
## ❓ Key Insights Queries

### 🏆 Highest CTR Day
```DAX
Top CTR Day =
TOPN(
    1,
    VALUES(ad_events[DayOfWeek]),
    [CTR],
    DESC
)
```
## 🕐 Best Time for Conversions
```DAX
Best Conversion Time =
TOPN(
    1,
    VALUES(ad_events[TimeOfDay]),
    [Total Conversions],
    DESC
)
```
## 👥 Audience Segmentation

### 📊 Matrix Setup

**Rows:**
- users[gender]  
- users[Age Group]  

**Values:**
- Total Impressions  
- Total Clicks  
- CTR  
- Total Conversions  

---

### 🏆 Top Segments (CTR)
```DAX
Top Segment CTR =
TOPN(
    3,
    SUMMARIZE(
        users,
        users[gender],
        users[Age Group],
        "CTR_Value", [CTR]
    ),
    [CTR],
    DESC
)
```
### 🏆 Top Segments (Conversion Rate)
```DAX
Top Segment Conversion =
TOPN(
    3,
    SUMMARIZE(
        users,
        users[gender],
        users[Age Group],
        "ConvRate", [Conversion Rate]
    ),
    [Conversion Rate],
    DESC
)
```
Worst Combo =
TOPN(
    1,
    SUMMARIZE(
        ads,
        ads[adplatform],
        ads[adtype],
        "CTR_Value", [CTR]
    ),
    [CTR],
    ASC
)
```
### ⚠️ Worst Performing Combination
```DAX
Worst Combo =
TOPN(
    1,
    SUMMARIZE(
        ads,
        ads[adplatform],
        ads[adtype],
        "CTR_Value", [CTR]
    ),
    [CTR],
    ASC
)
```
## 📊 Dashboard Design

### 🎯 KPI Cards
- Total Impressions  
- Total Clicks  
- CTR  
- Total Conversions  

---

### 📈 Charts
- CTR by DayOfWeek  
- Conversions by TimeOfDay  

---

### 🎛️ Slicers
- Platform → ads[adplatform]  
- Campaign → campaigns[campaign_name]  
- Country → users[country]  

---

## 🧰 Tools Used
- Power BI  
- DAX  
- Data Modeling  
- Data Visualization  

---

## 📌 Key Learnings
- Built end-to-end Power BI dashboard  
- Applied DAX for KPI calculations  
- Performed audience segmentation  
- Designed interactive reports  
