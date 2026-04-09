# 🌞 Sunshine Events Excel Project

## 📌 About the Project
Sunshine Events organizes cultural and musical events.  
This project analyzes event data (June–August 2021) using Excel formulas, pivot tables, and formatting.

---

## 📂 Worksheets in the File

- **Events** → Event details (City, Date, Tickets, etc.)
- **Descriptions** → Ticket category descriptions
- **Margins** → Profit margins based on ticket prices

---

## 🎯 Tasks & Solutions

---

### 🔹 1. Add Description & Working Days

#### ✅ Description Column
```excel
=XLOOKUP([@Category], Descriptions!A:A, Descriptions!B:B, "Not Found")
```
## ⚙️ Excel Implementation Steps

---

### ✅ Days Column (Working Days)

```excel
=NETWORKDAYS(TODAY(), [@Event_Date]-1)
```
## 📊 Pivot Table & Sales Analysis

---

### 🔹 2. Create Pivot Table

📊 **Sheet Name:** `Pivot`

#### ⚙️ Configuration

- **Rows:**
  - Event Date *(Group by Month)*
  - Genre  
- **Columns:**
  - Ticket Category  
- **Values:**
  - Sum of Tickets Sold  
- **Filters:**
  - City  

---

### 🔹 3. Calculate Margins & Sales

#### ✅ Margin (%)

```excel
=IF([@Price]="","N.A.", XLOOKUP([@Price], Margins!A:A, Margins!B:B))
```

#### ✅ Margin (Euros)

```excel
=[@Tickets_Sold] * [@[Margin (%)]] * [@Price]
```
### 🔹 4. Ticket Code & Rating

#### 🎟️ Ticket Code

```excel
=TEXT(YEAR([@Event_Date]),"00") & "-" & 
[@Category] & 
LEFT([@Event_Name],3) & "-" & 
LEFT([@City],3) & 
TEXT([@Event_Date],"d/m")
```
📌 **Example Output:**
```text
21-SC1AID-VIE10/6
```
### ⭐ Rating (Data Validation)

- **Allow →** Whole Number  
- **Between →** 1 and 10  

🚨 **Error Message:**
```text
Warning! You can only enter a number between 1 and 10
```
### 🔹 5. Calculations Sheet

📄 **Sheet Name:** `Calculations`

#### ✅ Total Margin (July)

```excel
=SUMIFS(Events!Margin_Euros, Events!Event_Date, ">=01-07-2021", Events!Event_Date, "<=31-07-2021")
```
#### ✅ Average Sales % (July)

```excel
=AVERAGEIFS(Events!Sales_Percentage, Events!Event_Date, ">=01-07-2021", Events!Event_Date, "<=31-07-2021")
```
#### ✅ Total Margin by Genre

```excel
=SUMIFS(Events!Margin_Euros, Events!Genre, A2, Events!Event_Date, ">=01-07-2021", Events!Event_Date, "<=31-07-2021")
```
### 🔹 6. Conditional Formatting

Apply to **Ticket Code column**:

```excel
=AND([@Tickets_Sold]>=500, [@Event_Date]>=DATE(2021,7,21))
```
🎨 **Highlight when:**

- Tickets ≥ 500  
- Event Date ≥ 21 July 2021  

---

### 🔹 7. Final Formatting

#### ✔ Apply:
- Table formatting  
- Bold headers  
- Auto column width  
- Date format → `DD-MMM-YYYY`  
- Percentage format  

#### ✔ Ensure:
- Clean layout  
- High readability  
- Professional appearance  

---

## 🚀 Key Skills Demonstrated

- 📊 Excel Formulas *(XLOOKUP, SUMIFS, AVERAGEIFS)*  
- 📈 Pivot Tables & Data Modeling  
- 🧹 Data Cleaning & Transformation  
- 🎨 Conditional Formatting  
- ✅ Data Validation  
- 📊 Analytical Thinking  

---

## 📊 Project Outcome

- ⚡ Automated calculations using dynamic formulas  
- 📊 Interactive insights via pivot tables  
- 🎯 Clean and professional Excel reporting  

---
