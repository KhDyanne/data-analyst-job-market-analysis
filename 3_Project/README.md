
#  Overview

Welcome to my analysis of the data job market with a focus on data analyst roles. This project was driven by a personal goal to better understand and navigate job opportunities in the data field.

Using data provided in Luke Barousse’s Python course, I conducted an in-depth exploration of job titles, salaries, locations, and in-demand skills. The dataset served as a foundation for answering critical career-related questions using Python.


# Questions

1. What are the most essential skills required for the leading three job titles in the data field?

2. How are in-demand skills trending for Data Analysts?

3. How well do jobs and skills pay for Data Analysts?

4. What are the most optimal skills to learn for Data Analysts?


# Tools Used

**Python**:   Served as the backbone of my analysis, enabling me to explore the dataset and extract meaningful insights. I used several key Python libraries:

- **Pandas** – Used for data cleaning, manipulation, and analysis.  
- **Matplotlib** – Provided tools for basic visualizations and plotting.  
- **Seaborn** – Enhanced the visuals with more advanced and aesthetically pleasing charts.

**Jupyter Notebooks** : Used for running Python scripts while combining code, notes, and analysis in one place.  
**Visual Studio Code** : My preferred editor for writing and executing Python code efficiently.  
**Git & GitHub** : Used for version control, collaboration, and tracking project progress.

# Data Cleanup

## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```
import ast
import pandas as pd
from datasets import load_dataset
import matplotlib.pyplot as plt

dataset=load_dataset('lukebarousse/data_jobs')
df=dataset['train'].to_pandas()

df['job_posted_date']=pd.to_datetime(df['job_posted_date'])
df['job_skills']=df['job_skills'].apply(lambda i: ast.literal_eval(i) if pd.notna(i) else i)
```
## Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```
df_US = df[df['job_country'] == 'United States']
```

# The Analysis
Here's how I answered each question:

# 1. What are the most essential skills required for the leading three job titles in the data field?

To find most essential skills for leading 3 data jobs, I filtered out those positions by which ones were the most popular, and got top 5 skills fot these top 3 roles.

### Visualization
``` 
fig, ax=plt.subplots(3,1)

for i, job_title in enumerate(job_titles):
    df_plot=df_skills_count[df_skills_count['job_title_short']==job_title].head()
    df_plot.plot(kind='barh', x='job_skills', y='skill_count', ax=ax[i], title=job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0,120000)
    
fig.suptitle('Counts of Top Skills in Job Postings', fontsize=15)
fig.tight_layout() 

```
### Result
![Most Demanded Skills](<../Photo/Most Demanded Skills.png>)

_Bar graph visualizing the top 3 data roles and their top 5 skills associated with each._

##  Insights

- **Python** and **SQL** are consistently in high demand across all three roles, making them essential foundational skills for any data professional.  
- For **Data Scientists**, Python tops the list, followed by SQL and R, highlighting the importance of programming and statistical analysis in this role.  
- **Data Engineers** heavily rely on SQL and Python, with notable mentions of cloud platforms like AWS and Azure, suggesting a strong focus on data infrastructure and pipeline development.  
- **Data Analysts** most frequently require SQL, followed by Excel and Python, indicating a balance between traditional spreadsheet tools and programming languages.  
- Visualization tools such as **Tableau** and **Power BI** are commonly mentioned for both Data Scientists and Data Analysts, showing the value of effectively presenting data insights.


# 2. How are in-demand skills trending for Data Analysts?
To find trending skills for Data Analysts in 2023, I filtered out data analyst positions and found the job postings by grouping the skills by month. This helped me to get top 5 data analyst skills by month, showcasing popularity of skills in 2023


### Visualization
```
df_plot=df_DA_US_percent.iloc[:,:5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```
### Result
![DA Top Skills](<../Photo/Top Skills for DA.png>)

_Line graph visualizing trending skills for Data Analysts in the US in 2023._

## Insights

- SQL remained the most in-demand skill throughout 2023, appearing in over half of all job postings.
- Excel held steady but showed a noticeable dip in demand during the last quarter before recovering in December.
- Python and Tableau had similar trends, while SAS consistently had the lowest demand among the top 5 skills.

# 3. How well do jobs and skills pay for Data Analysts?
To determine the highest-paying roles and skills, I focused on jobs located in the United States and examined their median salaries. I analyzed the salary distributions of popular data roles—such as Data Scientist, Data Engineer, and Data Analyst—to understand which positions tend to offer the highest compensation.

### Visualization
```
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

plt.title('Salary Distribution in the United States')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0,600000)

ax=plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.show()
```

### Result
![Sales Distribution](<../Photo/Sales Distribution.png>)
_Boxplot visualizing salary distributions for top 6 data jobs._

## Insights
- Senior Data Scientist roles command the highest salary potential—reaching up to $600K—highlighting the premium placed on advanced expertise and experience.

- Both Senior Data Engineer and Senior Data Scientist positions display numerous high-end outliers, indicating that exceptional skills or circumstances can lead to significantly elevated compensation. Meanwhile, Data Analyst salaries tend to be more consistent, with fewer extreme variations.

- Median salaries rise with both seniority and specialization. Senior-level roles not only offer higher typical pay but also exhibit greater variability, reflecting broader differences in compensation tied to responsibility and experience.

# 4. What are the most optimal skills to learn for Data Analysts?
To find the most optimal skills for data analysts to learn, I calculated the percent of skill demand and their median salary.

### Visualization

```
from adjustText import adjust_text

df_DA_skills_high_demand.plot(kind='scatter', x='skill_percent', y='median_salary')

texts=[]
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle="->", color='gray', lw=1))

from matplotlib.ticker import PercentFormatter
ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.tight_layout()

plt.show()
```

### Result
![Optimal Skills](<../Photo/Optimal Skills.png>)

_Scatter plot visualizing most optimal skills for data analysts._


# What I Learned

This project deepened my understanding of the data analyst job market and sharpened my technical skills in Python, particularly in data manipulation and visualization. Key takeaways include:

- **Advanced Python Skills**: I strengthened my use of libraries like Pandas for data processing, and Seaborn/Matplotlib for effective visualization, enabling efficient exploration of complex datasets.  
- **Data Cleaning Essentials**: I realized how crucial clean, well-structured data is—it's the foundation for drawing accurate and actionable insights.  
- **Skill Strategy**: By analyzing skill demand and salary trends, I learned how aligning my skillset with market needs can support smarter career planning.


# Insights

Through this exploration, I uncovered several valuable insights into the data analytics job landscape:

- **Skill vs. Salary**: In-demand skills like Python and Oracle tend to command higher salaries, showing a strong link between specialization and compensation.  
- **Market Dynamics**: The job market evolves constantly, and skill demand shifts with it. Staying updated with these trends is vital for long-term growth.  
- **Value-Driven Learning**: Focusing on skills that are both sought-after and high-paying can guide learning paths and career decisions for aspiring analysts.



# Challenges I Faced

While rewarding, the project also brought a few challenges that became learning moments:

- **Data Quality**: Managing missing or inconsistent data required careful cleaning to maintain analysis reliability.  
- **Complex Visuals**: Creating intuitive visualizations from multifaceted data sets was tough but essential for delivering clear insights.  
- **Breadth vs. Depth**: Striking the right balance between broad analysis and deep dives was a continuous effort to ensure both coverage and clarity.


#  Conclusion

This project offered meaningful insights into the evolving role of data analysts and the skills that drive value in the job market. It served as a strong foundation for future exploration while emphasizing the importance of staying adaptable and continuously learning. As the data landscape keeps evolving, so must the skills and strategies we bring to it.
