# Python Analysis of the U.S. Data Engineer Job Market

> This project explores the **U.S. Data Engineer job market** using **Python** to identify the **most in-demand skills**, examine **how skill demand changes over time**, analyze **salary trends**, and discover the skills that offer the **best balance between demand and pay**.

---
# Overview

As part of my Python learning journey, I wanted to apply what I had learned to a real-world dataset instead of only following coding exercises. This project explores the **U.S. Data Engineer job market** using **Python** to identify **in-demand skills**, examine **how skill demand changes over time**, analyze **salary trends**, and discover the skills that offer the **best balance between demand and pay**.

The analysis was conducted in **Jupyter Notebooks** using **Pandas** for data manipulation and **Matplotlib** and **Seaborn** for data visualization. By cleaning, exploring, and visualizing the dataset, this project transforms raw job posting data into practical insights that help illustrate the demand for skills in the U.S. data engineer job market.

---
# Objectives

This project aims to answer the following questions:

1. What skills are most in demand for the top three data roles in the **U.S.** job market?
2. How does demand for **Data Engineer skills** change over time?
3. Which are the **highest-paying skills**, and how do the **most in-demand skills** compare by yearly median salary?
4. Which skills offer the best combination of **high demand** and **high salary** for Data Engineers?

---
# Dataset

**Source**

The dataset used in this project is the **Data Jobs Dataset** by **Luke Barousse**, available on **[Hugging Face](https://huggingface.co/datasets/lukebarousse/data_jobs)**.

**Description**

The dataset contains job posting data from the **U.S.** data job market, including information about **job titles**, **companies**, **locations**, **salary information**, **required skills**, and **posting dates**.

**Scope**

This analysis focuses on **Data Engineer** roles within the **U.S.** job market. While the dataset includes multiple data-related professions, this project focuses specifically on Data Engineer roles to answer the objectives outlined above.

---
# Tools & Technologies

- **Python** – Core programming language used for data analysis.
- **Pandas** – Data cleaning, transformation, and analysis.
- **Matplotlib** – Data visualization.
- **Seaborn** – Statistical data visualization.
- **Jupyter Notebook** – Interactive environment for analysis and documentation.
- **Visual Studio Code** – Development environment.
- **Git & GitHub** – Version control and project hosting.

---
# Project Workflow

```text
Dataset
    │
    ▼
Data Cleaning & Transformation
    │
    ▼
Exploratory Data Analysis (EDA)
    │
    ▼
Data Visualization
    │
    ▼
Insights
```

---
# Analysis

This section presents the exploratory data analysis performed to answer the project objectives. Each analysis includes the objective, the analytical approach, the resulting visualization, and the key insights.

---
## 1. Most In-Demand Skills

### Objective

Identify the skills most frequently requested for the top three data roles in the **U.S.** job market.

> **View the complete analysis and Python implementation:** [`01_Skills_Demand.ipynb`](/notebooks/01_Skills_Demand.ipynb)

### Visualization

![Most In-Demand Skills](/images/01_Skills_Demand.png)

> *The snippet below highlights the core visualization logic. See the notebook (.ipynb) above for the complete implementation.*

```python
job_titles = df['job_title_short'].value_counts().head(3).index.to_list()
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_titles in enumerate(job_titles):
    df_plot = df_skills_pct[df_skills_pct['job_title_short'] == job_titles].head(5)
    sns.barplot(
        data=df_plot, 
        x='skill_pct', 
        y='job_skills', 
        ax=ax[i], 
        hue='skill_pct', 
        palette='dark:b_r'
    )
    ax[i].set_title(job_titles)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 75)

    # here's the label
    for n, v in enumerate(df_plot['skill_pct']):
        ax[i].text(v, n, f'{v: .0f}%', va='center', fontsize=8) 

    ax[i].set_xticks([])   

fig.suptitle('Likelihood Skill Requested in US Data Job Postings ', fontsize=14)
fig.tight_layout()

plt.show()
```

### Insights

- **SQL** is the most in-demand skill for Data Analysts (50.8%) and Data Engineers (68.3%), while **Python** ranks highest for Data Scientists (72.0%).
- **Python** is consistently one of the most requested skills across all three roles, appearing in more than 27% of Data Analyst, 64.9% of Data Engineer, and 72.0% of Data Scientist job postings.
- Data Analysts place greater emphasis on **Excel** and **Tableau**, highlighting the importance of reporting and visualization tools.
- Data Engineers show strong demand for **AWS**, **Azure**, and **Spark**, reflecting the importance of cloud and big data technologies.
- While **SQL** and **Python** are common across all three roles, the remaining in-demand skills differ based on the technical focus of each position.

---
## 2. Skill Demand Trends

### Objective

Analyze how demand for the top Data Engineer skills changes throughout the year in the **U.S.** job market.

> **View the complete analysis and Python implementation:** [`02_Skill_Trends.ipynb`](/notebooks/02_Skill_Trends.ipynb)

### Visualization

![Skill Demand Trends](/images/02_Skill_Trends.png)

> *The snippet below highlights the core visualization logic. See the notebook (.ipynb) above for the complete implementation.*

```python
df_plot = df_de_pct.iloc[:, :5]

sns.lineplot(
    data=df_plot,
    dashes=False,
    palette='tab10'
)

sns.despine()

plt.title('Top Skills Trend of Data Engineer in U.S.')
plt.xlabel('2023')
plt.ylabel('Likelihood (%)')
plt.ylim(25, 75)
plt.yticks(np.arange(25, 76, 5))
plt.legend().set_visible(False)

# Format y-axis as percentages
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

# Add skill names to the end of each line
for i in range(5):
    plt.text(
        11.2,
        df_plot.iloc[-1, i],
        df_plot.columns[i],
        fontsize=8
    )

plt.show()
```

### Insights

- **SQL** remained the most requested skill throughout 2023, consistently appearing in around **65–71%** of Data Engineer job postings.
- **Python** maintained the second-highest demand, closely following SQL for most of the year despite a noticeable decline in September.
- **AWS** consistently ranked as the third most requested skill, with demand generally remaining between **39%** and **46%**.
- **Azure** and **Spark** showed similar levels of demand, fluctuating around **30–34%** without significant changes over the year.
- All five skills experienced a dip in demand around **September**, before partially recovering during the final months of the year.

---
## 3. Highest-Paying Skills vs. Most In-Demand Skills

### Objective

Compare the **highest-paying technical skills** with the **most in-demand skills** based on yearly median salary for **Data Engineers** in the **U.S.** job market.

> **View the complete analysis and Python implementation:** [`03_Skills_Salary.ipynb`](/notebooks/03_Skills_Salary.ipynb)

### Visualization

![Highest-Paying Skills vs. Most In-Demand Skills](/images/03_Skills_Salary.png)

> *The snippet below highlights the core visualization logic. See the notebook (.ipynb) above for the complete implementation.*

```python
fig, ax = plt.subplots(2, 1)

# Plot top 10 highest-paying skills
sns.barplot(
    data=df_top_pay,
    x='median',
    y=df_top_pay.index,
    ax=ax[0],
    hue='median',
    palette='dark:b_r'
)

# Plot top 10 most in-demand skills
sns.barplot(
    data=df_top_skills,
    x='median',
    y=df_top_skills.index,
    ax=ax[1],
    hue='median',
    palette='dark:b_r'
)

sns.despine()

fig.suptitle('Data Engineer in United States', fontsize=14)
fig.tight_layout()

ax[0].set_title('Top 10 Highest-Paying Skills')
ax[0].legend().remove()
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].xaxis.set_major_formatter(
    plt.FuncFormatter(lambda x, _: f'${int(x / 1000)}K')
)

ax[1].set_title('Top 10 Most In-Demand Skills')
ax[1].legend().remove()
ax[1].set_xlabel('Yearly Median Salary (USD)')
ax[1].set_ylabel('')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(
    plt.FuncFormatter(lambda x, _: f'${int(x / 1000)}K')
)

plt.show()
```

### Insights

- **Mongo** has the highest yearly median salary at approximately **$208K**, followed by **Vue**, **Solidity**, and **Node**.
- Several of the highest-paying skills, including **Vue**, **Solidity**, **ggplot2**, and **OpenCV**, appear in very few job postings, indicating that high salaries do not necessarily correspond to high demand.
- Among the most in-demand skills, **Kafka** offers the highest yearly median salary (**$145K**), followed by **NoSQL** (**$140K**) and **Spark** (**$137K**).
- Core Data Engineer technologies such as **Python**, **SQL**, **AWS**, and **Azure** remain among the most requested skills, with yearly median salaries ranging from approximately **$125K** to **$131K**.
- The comparison highlights that the **highest-paying skills** and the **most in-demand skills** are not always the same, illustrating the trade-off between salary potential and market demand.

---
## 4. Most Optimal Skills

### Objective

Identify the technical skills that offer the best combination of **high demand** and **high yearly median salary** for **Data Engineers** in the **U.S.** job market.

> **View the complete analysis and Python implementation:** [`04_Optimal_Skills.ipynb`](/notebooks/04_Optimal_Skills.ipynb)

### Visualization

![Most Optimal Skills](/images/04_Optimal_Skills.png)

> *The snippet below highlights the core visualization logic. See the notebook (.ipynb) above for the complete implementation.*

```python
sns.scatterplot(
    data=df_plot,
    x='skill_pct',
    y='median_salary',
    hue='technology'
)

sns.set_theme(style='ticks')
sns.despine()

plt.title('Most Optimal Skills for Data Engineer')
plt.xlabel('% Likelihood Skills in Job Posting')
plt.ylabel('Median Yearly Salary (USD)')

# Format the x-axis and y-axis
ax = plt.gca()
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
ax.yaxis.set_major_formatter(
    plt.FuncFormatter(lambda y, _: f'${int(y / 1000)}K')
)

# Add label to each dot
texts = []

for i, skill in enumerate(df_skills.index):
    texts.append(
        plt.text(
            df_skills['skill_pct'].iloc[i],
            df_skills['median_salary'].iloc[i],
            skill
        )
    )

# Adjust text to avoid overlap and add arrows
adjust_text(
    texts,
    arrowprops={'arrowstyle': '->', 'color': 'gray'}
)

plt.show()
```

### Insights

- **Python** and **SQL** offer the strongest balance of demand and salary, appearing in over **68%** of Data Engineer job postings while providing yearly median salaries above **$125K**.
- **AWS** combines high demand (45.0%) with a competitive yearly median salary of approximately **$131K**, making it one of the most valuable cloud skills for Data Engineers.
- **Kafka** provides the highest yearly median salary (**$145K**) among the skills shown, although it appears in fewer than **20%** of job postings.
- **Spark** and **Java** stand out by offering both above-average salaries (around **$137K**) and moderate market demand.
- The visualization shows that the most optimal skills are not always the highest-paying ones, but those that balance **market demand** with **salary potential**.
---
## Key Takeaways

- **SQL** and **Python** are the core skills for Data Engineers, consistently appearing as the most requested technologies across job postings.
- Cloud and big data technologies, particularly **AWS**, **Spark**, and **Azure**, play an important role in the Data Engineer skill set and are frequently requested by employers.
- The **highest-paying skills** are not always the **most in-demand**, highlighting the trade-off between salary potential and market demand.
- Skills such as **Python**, **SQL**, **AWS**, and **Spark** provide a strong balance of demand and salary, making them valuable priorities for aspiring Data Engineers.

---
## Limitations

- The analysis focuses exclusively on **Data Engineer** job postings in the **U.S.**, so the findings may not represent global hiring trends.
- Salary analysis is based on job postings that include salary information, meaning positions without reported salaries are excluded.
- The dataset reflects a specific period of job postings and may not capture changes in hiring demand over time beyond the available data.
- Skill demand is measured by the frequency of skills listed in job postings and does not account for differences in job seniority, industry, or company size.