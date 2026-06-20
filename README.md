# Bank Marketing Data Cleaning Project | مشروع تنظيف بيانات الحملات التسويقية للبنك

![Python](https://img.shields.io/badge/Python-3.13.6-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.24+-013243?logo=numpy&logoColor=white)
![DataCamp](https://img.shields.io/badge/DataCamp-Scholar-03EF62?logo=datacamp&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success)


##  نظرة عامة | Overview

هذا المشروع يهدف إلى تنظيف ومعالجة بيانات الحملات التسويقية لبنك بريطاني، حيث تم جمع البيانات كجزء من حملة تسويقية للحصول على عملاء جدد للحصول على قروض شخصية. تم تحويل البيانات غير المنظمة إلى ثلاثة ملفات CSV منظمة وجاهزة للاستيراد إلى قاعدة بيانات PostgreSQL.


This project aims to clean and process marketing campaign data for a British bank, where the data was collected as part of a marketing campaign to acquire new customers for personal loans. The unstructured data was transformed into three structured CSV files ready for import into a PostgreSQL database.

---
##  السياق التعليمي | Educational Context

This project was developed as part of my preparation for the **DataCamp Associate Data Scientist in Python** certification.

تم تطوير هذا المشروع كجزء من تحضيري للحصول على شهادة **DataCamp Associate Data Scientist in Python**.

---


##  ملفات المشروع | Project Files

| الملف | الوصف | Description |
|-------|-------|-------------|
| `bank_marketing.csv` | ملف البيانات الخام (المدخلات) | Raw data file (input) |
| `client.csv` | معلومات العملاء الأساسية | Basic client information |
| `campaign.csv` | بيانات الحملات التسويقية | Marketing campaign data |
| `economics.csv` | المؤشرات الاقتصادية | Economic indicators |
| `notebook.ipynb` | دفتر Jupyter مع كود المعالجة | Jupyter Notebook with processing code |

---

## أهداف المشروع | Project Objectives

1. تنظيف البيانات وتحويلها إلى الصيغة المطلوبة | Clean data and convert to required format
2. تغيير أنواع البيانات حسب المواصفات | Change data types according to specifications
3. تقسيم البيانات إلى ثلاثة ملفات منفصلة | Split data into three separate files:
   - معلومات العملاء | Client information
   - بيانات الحملة التسويقية | Campaign data
   - المؤشرات الاقتصادية | Economic indicators

---

##  التقنيات المستخدمة | Technologies Used

- **Python 3.13.6**
- **Pandas** - لمعالجة وتحليل البيانات | for data processing and analysis
- **NumPy** - للعمليات الحسابية | for numerical operations
- **Jupyter Notebook** - بيئة التطوير التفاعلية | Interactive development environment

---

##  هيكل البيانات النهائي | Final Data Structure

### 1. client.csv

| العمود | نوع البيانات | الوصف |
|--------|--------------|-------|
| client_id | integer | معرف العميل |
| age | integer | العمر بالسنوات |
| job | object | نوع الوظيفة |
| marital | object | الحالة الاجتماعية |
| education | object | المستوى التعليمي |
| credit_default | bool | هل العميل في حالة تخلف عن السداد |
| mortgage | bool | هل يوجد رهن عقاري |

| Column | Data Type | Description |
|--------|-----------|-------------|
| client_id | integer | Client identifier |
| age | integer | Age in years |
| job | object | Job type |
| marital | object | Marital status |
| education | object | Education level |
| credit_default | bool | Whether client is in default |
| mortgage | bool | Whether client has a mortgage |

### 2. campaign.csv

| العمود | نوع البيانات | الوصف |
|--------|--------------|-------|
| client_id | integer | معرف العميل |
| contact_duration | integer | مدة آخر اتصال بالثواني |
| number_contacts | integer | عدد محاولات الاتصال |
| previous_campaign_contacts | integer | عدد محاولات الاتصال في الحملة السابقة |
| previous_outcome | bool | نتيجة الحملة السابقة |
| campaign_outcome | bool | نتيجة الحملة الحالية |
| last_contact_date | datetime | تاريخ آخر اتصال |

| Column | Data Type | Description |
|--------|-----------|-------------|
| client_id | integer | Client identifier |
| contact_duration | integer | Duration of last contact in seconds |
| number_contacts | integer | Number of contact attempts |
| previous_campaign_contacts | integer | Number of contact attempts in previous campaign |
| previous_outcome | bool | Previous campaign outcome |
| campaign_outcome | bool | Current campaign outcome |
| last_contact_date | datetime | Date of last contact |

### 3. economics.csv

| العمود | نوع البيانات | الوصف |
|--------|--------------|-------|
| client_id | integer | معرف العميل |
| cons_price_idx | float | مؤشر أسعار المستهلك |
| euribor_three_months | float | سعر يوريبور لثلاثة أشهر |

| Column | Data Type | Description |
|--------|-----------|-------------|
| client_id | integer | Client identifier |
| cons_price_idx | float | Consumer price index |
| euribor_three_months | float | 3-month Euribor rate |

---

##  عمليات التنظيف والمعالجة | Cleaning & Processing Operations

### التحديات والحلول | Challenges & Solutions

#### 1. استبدال النقاط (.) في النصوص | Replacing Dots (.) in Text

**المشكلة | Problem:**
وجود نقاط في قيم الأعمدة مثل `admin.` و `basic.4y` مما قد يسبب مشاكل في قواعد البيانات.

Dots present in column values such as `admin.` and `basic.4y`, which could cause issues in databases.

**الحل | Solution:**
استخدام `str.replace('.', '_')` لاستبدال النقاط بشرطة سفلية.

Using `str.replace('.', '_')` to replace dots with underscores.

```python
df['job'] = df['job'].str.replace(".", "_")
df['education'] = df['education'].str.replace('.', '_')
```

---

#### 2. تحويل القيم المنطقية | Converting Boolean Values

**المشكلة | Problem:**
البيانات النصية مثل `"yes"`, `"no"`, `"unknown"`, `"success"`, `"failure"`, `"nonexistent"` تحتاج للتحويل إلى قيم منطقية (boolean) لضمان التوافق مع قواعد البيانات.

Textual data such as `"yes"`, `"no"`, `"unknown"`, `"success"`, `"failure"`, `"nonexistent"` needed to be converted to boolean values for database compatibility.

**الحل | Solution:**
استخدام `np.where()` مع التحويل إلى `bool`.

Using `np.where()` with conversion to `bool`.

```python
df['credit_default'] = np.where(df['credit_default'] == 'yes', 1, 0).astype(bool)
df['campaign_outcome'] = np.where(df['campaign_outcome'] == 'yes', 1, 0).astype(bool)
df['previous_outcome'] = np.where(df['previous_outcome'] == "success", 1, 0).astype(bool)
```

---

#### 3. معالجة القيم المفقودة | Handling Missing Values

**المشكلة | Problem:**
قيمة `"unknown"` في عمود التعليم تمثل بيانات مفقودة ويجب تحويلها إلى `NaN` للتعامل معها بشكل صحيح.

The value `"unknown"` in the education column represents missing data and needed to be converted to `NaN` for proper handling.

**الحل | Solution:**
استخدام `replace('unknown', np.nan)`.

Using `replace('unknown', np.nan)`.

```python
df['education'] = df['education'].replace('unknown', np.nan)
```

---

#### 4. إنشاء تاريخ الاتصال | Creating the Contact Date

**المشكلة | Problem:**
توفر البيانات أيام وشهور منفصلة بدون سنة، مما يتطلب دمجها في عمود تاريخ واحد.

The data contained separate day and month columns without a year, requiring them to be merged into a single date column.

**الحل | Solution:**
- إنشاء عمود سنة بقيمة ثابتة `2022`
- تحويل أسماء الأشهر إلى أرقام باستخدام قاموس
- دمج الأعمدة في تاريخ واحد باستخدام `pd.to_datetime()`

- Creating a year column with a fixed value of `2022`
- Converting month names to numbers using a dictionary
- Merging columns into a single date using `pd.to_datetime()`

```python
df['year'] = 2022
month_to_num = {
    'jan': 1, 'feb': 2, 'mar': 3, 'apr': 4,
    'may': 5, 'jun': 6, 'jul': 7, 'aug': 8,
    'sep': 9, 'oct': 10, 'nov': 11, 'dec': 12
}
df['month'] = df['month'].map(month_to_num)
df['last_contact_date'] = pd.to_datetime(df[['year', 'month', 'day']])
```

---

##  نتائج المعالجة | Processing Results

- **إجمالي عدد السجلات | Total Records:** 41,188
- **عدد الأعمدة في الملفات النهائية | Columns in Final Files:**
  - client.csv: 7 أعمدة | 7 columns
  - campaign.csv: 7 أعمدة | 7 columns
  - economics.csv: 3 أعمدة | 3 columns
- **نوع البيانات | Data Types:** تم تحويل جميع الأعمدة إلى الأنواع المطلوبة | All columns converted to required types

---

##  الدروس المستفادة | Lessons Learned

1. **أهمية فهم البيانات | Importance of Understanding Data:**
   قراءة وفهم هيكل البيانات قبل البدء بالتنظيف يوفر الوقت ويقلل من الأخطاء.
   Reading and understanding data structure before cleaning saves time and reduces errors.

2. **التعامل مع البيانات النصية | Handling Text Data:**
   استخدام دوال pandas المناسبة لتنظيف النصوص وتوحيدها.
   Using appropriate pandas functions to clean and standardize text.

3. **التحويل بين أنواع البيانات | Data Type Conversion:**
   استخدام `astype()` و `np.where()` للتحويلات المنطقية والعددية.
   Using `astype()` and `np.where()` for boolean and numeric conversions.

4. **إنشاء أعمدة جديدة | Creating New Columns:**
   دمج عدة أعمدة لإنشاء معلومات مفيدة (مثل تاريخ الاتصال من اليوم والشهر).
   Merging multiple columns to create useful information (e.g., contact date from day and month).

5. **تقسيم البيانات | Data Splitting:**
   فصل البيانات إلى ملفات متعددة حسب الغرض لتسهيل الاستعلام والتحليل.
   Separating data into multiple files by purpose to facilitate querying and analysis.

---

##  كيفية تشغيل المشروع | How to Run the Project

### 1. تثبيت المتطلبات | Install Requirements

```bash
pip install pandas numpy
```

### 2. تشغيل Jupyter Notebook | Run Jupyter Notebook

```bash
jupyter notebook notebook.ipynb
```

### 3. قم بتشغيل جميع الخلايا بالتسلسل | Run all cells sequentially

---

## 📝 ملاحظات إضافية | Additional Notes

- تم استخدام `copy()` لتجنب تعديل البيانات الأصلية | `copy()` was used to avoid modifying original data
- تم الحفاظ على معرف العميل (`client_id`) كمفتاح أساسي لربط الملفات | `client_id` was preserved as a primary key to link files
- جميع الملفات النهائية تم حفظها بصيغة CSV بدون فهرس | All final files were saved as CSV without index
- تم تصميم البيانات النهائية للاستيراد المباشر إلى PostgreSQL | Final data is designed for direct import into PostgreSQL

---

## 📂 هيكل المشروع | Project Structure

```
bank-marketing-data-cleaning/
│
├── 
│   ├── bank_marketing.csv    # البيانات الخام | Raw data
│   ├── client.csv            # معلومات العملاء | Client info
│   ├── campaign.csv          # بيانات الحملة | Campaign data
│   └── economics.csv         # المؤشرات الاقتصادية | Economic indicators
│
├── notebook.ipynb            # دفتر Jupyter | Jupyter Notebook            
└── README.md     # هذا الملف | This file
```

---

##  متطلبات المشروع | Project Requirements

```
pandas>=2.0.0
numpy>=1.24.0
```

## 🎓 شكر وتقدير | Acknowledgments

شكراً لـ **DataCamp** على المنحة، ولمبادرة **Engineering Tunisia** على الدعم.

Thanks to **DataCamp** for the scholarship, and to the **Engineering Tunisia** initiative for their support.

---

