
# 🌍 World Layoffs Exploratory Data Analysis Project

This project is a comprehensive exploratory data analysis (EDA) of a global layoffs dataset. The goal is to uncover meaningful insights into layoff trends across industries, regions, and companies using advanced SQL techniques.

---

## 🛠 Project Objectives

1. **Understand Trends**: Analyze layoffs over time to identify patterns.  
2. **Identify Key Players**: Highlight companies and industries most affected.  
3. **Explore Insights**: Use advanced SQL features like window functions and Common Table Expressions (CTEs) for deeper analysis.

---

## 📊 Key Insights and Analysis

### 1. Dataset Overview
Fetched all records for an initial understanding of the dataset.  
**Query:**
```sql
SELECT * 
FROM layoffs_staging2;
```

---

### 2. Summary Statistics
- **Maximum Layoffs:** Identified the highest layoffs and percentages.  
- **Date Range:** Determined the dataset's timeline.  
**Query:**
```sql
SELECT 
    MAX(total_laid_off) AS Max_Layoffs, 
    MAX(percentage_laid_off) AS Max_Percentage,
    MIN(`date`) AS Earliest_Date, 
    MAX(`date`) AS Latest_Date
FROM layoffs_staging2;
```

---

### 3. Company Analysis
- **Top Companies by Layoffs:** Ranked by total layoffs.  
- **Companies Laying Off 100% of Workforce:** Highlighted and sorted by funds raised.  
**Query:**
```sql
SELECT 
    company, 
    SUM(total_laid_off) AS Total_Layoffs
FROM layoffs_staging2
GROUP BY company
ORDER BY Total_Layoffs DESC;
```

---

### 4. Industry and Geography Trends
- **Industry Impact:** Aggregated layoffs by industry.  
- **Geographical Impact:** Explored country-wise layoffs.  
**Query:**
```sql
SELECT 
    industry, 
    SUM(total_laid_off) AS Total_Layoffs
FROM layoffs_staging2
GROUP BY industry
ORDER BY Total_Layoffs DESC;
```

---

### 5. Time-Based Analysis
- **Annual Trends:** Explored layoffs by year.  
- **Monthly Rolling Totals:** Computed rolling totals for layoffs using CTEs.  
**Query:**
```sql
WITH Rolling_Total AS (
    SELECT 
        SUBSTRING(`date`, 1, 7) AS `Month`, 
        SUM(total_laid_off) AS Total_Off
    FROM layoffs_staging2
    GROUP BY `Month`
    ORDER BY `Month`
)
SELECT 
    `Month`, 
    Total_Off,
    SUM(Total_Off) OVER (ORDER BY `Month`) AS Rolling_Total
FROM Rolling_Total;
```

---

### 6. Advanced Analysis
- **Top Companies Per Year:** Ranked companies with the most layoffs yearly using `DENSE_RANK`.  
**Query:**
```sql
WITH company_year AS (
    SELECT 
        company, 
        YEAR(`date`) AS Year, 
        SUM(total_laid_off) AS Total_Layoffs
    FROM layoffs_staging2
    GROUP BY company, Year
), company_year_ranking AS (
    SELECT 
        *, 
        DENSE_RANK() OVER (PARTITION BY Year ORDER BY Total_Layoffs DESC) AS Ranking
    FROM company_year
)
SELECT *
FROM company_year_ranking
WHERE Ranking <= 5;
```

---

## 🖥 Technologies Used

- **SQL**: Structured Query Language for querying and analyzing data.  
- **MySQL**: Database platform for executing and managing queries.  

---

## 🚀 How to Run

1. Clone this repository to your local machine.  
2. Load the SQL file into your MySQL database system.  
3. Execute the queries to reproduce the analysis.

---

## 🌟 Learnings

- Effective use of SQL for analyzing real-world datasets.  
- Advanced skills with window functions and CTEs for ranking and trends.  
- Aggregation techniques to summarize large datasets.

---

## 👤 Author

**Yasin Arfaat**  
- Data Analyst | SQL Enthusiast | EDA Specialist  

If you have any questions or suggestions, feel free to reach out! 🙌
