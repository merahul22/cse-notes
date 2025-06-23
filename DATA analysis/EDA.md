

## 1. Overview of EDA Workflow

1. **Data Loading**  
    Load the data into a DataFrame and inspect basic properties.
    
2. **Data Inspection & Cleaning**
    
    - Check dimensions, types, missing values.
        
    - Handle or flag missing / duplicate records.
        
3. **Univariate Analysis**
    
    - **Numeric features**: distributions, outliers, summary statistics.
        
    - **Categorical features**: frequency counts, bar plots.
        
4. **Bivariate Analysis**
    
    - Relationship between one feature and the target (or between two features).
        
    - Numeric–numeric: scatterplots, correlation.
        
    - Numeric–categorical: boxplots, group summaries.
        
    - Categorical–categorical: crosstabs, stacked bars.
        
5. **Multivariate Analysis**
    
    - Examine interactions among three or more variables.
        
    - Correlation matrices, heatmaps.
        
    - Pivot tables, layered plots.
        
6. **Feature Engineering & Next Steps**
    
    - Create new features if needed.
        
    - Select important variables for modeling.
        

---

## 2. Load & Inspect the Titanic Dataset

```python
import pandas as pd

# 1. Load
df = pd.read_csv('titanic.csv')  # or seaborn.load_dataset('titanic')

# 2. Quick look
print(df.shape)        # e.g. (891, 12)
print(df.dtypes)
print(df.head())
```

**Theory:**

- `.shape` tells you how many rows and columns.
    
- `.dtypes` reveals data types (numeric, object, etc.).
    
- `.head()` shows sample records to verify loading.
    

---

## 3. Data Cleaning & Missing Values

```python
# 1. Missing value count
missing = df.isna().sum().sort_values(ascending=False)
print(missing[missing > 0])

# 2. Decide strategy:
#    - Drop (if few)
#    - Impute (e.g., median for Age, mode for Embarked)
df['Age'].fillna(df['Age'].median(), inplace=True)
df['Embarked'].fillna(df['Embarked'].mode()[0], inplace=True)
```

**Theory:**

- You must identify where data are missing.
    
- Choose an imputation or removal strategy that makes sense for your analysis or modeling.
    

---

## 4. Univariate Analysis

### 4.1 Numeric Features

```python
import matplotlib.pyplot as plt

# Example: Age distribution
plt.figure()
plt.hist(df['Age'], bins=30)
plt.title('Age Distribution')
plt.xlabel('Age')
plt.ylabel('Count')
plt.show()

# Summary statistics
print(df['Age'].describe())
```

**Theory:**

- Histograms reveal central tendency, spread, skewness, and outliers.
    
- `.describe()` gives count, mean, std, quartiles.
    

### 4.2 Categorical Features

```python
# Example: Count of passengers by class
class_counts = df['Pclass'].value_counts().sort_index()
print(class_counts)

# Bar plot
plt.figure()
plt.bar(class_counts.index.astype(str), class_counts.values)
plt.title('Passenger Class Counts')
plt.xlabel('Pclass')
plt.ylabel('Count')
plt.show()
```

**Theory:**

- Bar plots show frequencies for each category.
    
- Good to check class imbalance or dominant categories.
    

---

## 5. Bivariate Analysis

### 5.1 Numeric vs. Numeric

```python
# Fare vs. Age scatter
plt.figure()
plt.scatter(df['Age'], df['Fare'], alpha=0.5)
plt.title('Fare vs. Age')
plt.xlabel('Age')
plt.ylabel('Fare')
plt.show()

# Correlation coefficient
print(df[['Age','Fare']].corr())
```

**Theory:**

- Scatterplots visualize linear/non‑linear relationships.
    
- Pearson correlation quantifies linear association.
    

### 5.2 Numeric vs. Categorical

```python
# Age distribution by survival
survived = df[df['Survived']==1]['Age']
died     = df[df['Survived']==0]['Age']

plt.figure()
plt.boxplot([died, survived], labels=['Died','Survived'])
plt.title('Age by Survival Status')
plt.ylabel('Age')
plt.show()

print(df.groupby('Survived')['Age'].agg(['mean','median']))
```

**Theory:**

- Boxplots reveal differences in distribution across groups.
    
- Grouped summaries help compare central tendencies.
    

### 5.3 Categorical vs. Categorical

```python
# Survival rate by Pclass
ct = pd.crosstab(df['Pclass'], df['Survived'], normalize='index')
print(ct)

# Stacked bar chart
ct.plot(kind='bar', stacked=True)
plt.title('Survival Rate by Pclass')
plt.xlabel('Pclass')
plt.ylabel('Proportion')
plt.legend(title='Survived')
plt.show()
```

**Theory:**

- Crosstabs show joint frequency or proportion tables.
    
- Stacked bars help visualize composition differences.
    

---

## 6. Multivariate Analysis

```python
# 1. Correlation matrix for numeric features
corr = df[['Survived','Age','Fare','SibSp','Parch']].corr()

# 2. Heatmap (using matplotlib)
plt.figure(figsize=(6,5))
plt.imshow(corr, interpolation='none', aspect='auto')
plt.colorbar()
plt.xticks(range(len(corr)), corr.columns, rotation=45)
plt.yticks(range(len(corr)), corr.index)
plt.title('Correlation Matrix')
plt.show()

# 3. Pivot: survival rate by class and sex
pivot = df.pivot_table(index='Pclass', columns='Sex', values='Survived')
print(pivot)
```

**Theory:**

- Correlation matrices help identify pairs of features that move together.
    
- Pivot tables let you explore two‑way interactions in depth.
    

---

## 7. Feature Engineering & Next Steps

- **Derive** new features: e.g., FamilySize = SibSp + Parch + 1.
    
- **Transform** skewed distributions (log‑transform Fare).
    
- **Encode** categoricals (one‑hot for Embarked, Sex).
    
- **Select** features based on domain knowledge and EDA findings.
    

```python
df['FamilySize'] = df['SibSp'] + df['Parch'] + 1
df['Fare_log']  = df['Fare'].apply(lambda x: np.log1p(x))
```

---
 FOR DETAIL ANALYSIS       [GOOGLE COLLAB-EDA](https://colab.research.google.com/drive/13rFqQJqU5RgxSdtUARZAUrzAoweE3rbQ?usp=sharing#scrollTo=TaViKg2OMAED)
 