# Overview

This project is an analysis of the job market, focusing on data analyst roles but also looking into other data related roles. This project was created to showcase my ability to perform analysis of data using Python coding however I also wanted to understand the job market more effectively. In this project I focus around the top-paying and most in-demand job titles and skills to find the most optimal jobs for my circumstances.

The files you will see in this repository are Python notebooks I used to practice some python code. The data I used to perform my analysis comes from [Luke Barousse's Python Course](https://huggingface.co/datasets/lukebarousse/data_jobs), this course helped me to explore and understand the data. Through this project I was able to explore key question such as the most in-demand skills, how salaries trend over the year and how demand and salary compare in data analytics. Although the data used is from the year of 2023 I believe the insights I gained were crucial to my understanding of the job market and helped me to not only practice my python skills but showcase them to you.

# Questions

These are the questions I wanted to answer for my project:

1. What are the most demanded skills for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the most optimal skills for Data Analysts to learn?

# Tools I Used

-  **Python:** The foundation of my analysis, allowing me to analyse the data and find insights critical for my understanding of the data.  
I also used the following Python libraries:

    - **Pandas Library:** This was used for formatting and analysing the data.
    - **Matplotlib Library:** This was used to visualise the data ready for analysis.
    - **Seaborn Library:** This was used to help me create more advanced visualisations that were visually appealing.

- **Visual Studio Code:** The IDE I used to execute my Python scripts
- **Jupyter Notebooks:** The tool I used to write and run my Python scripts that allowed me to showcase my code.
- **Git & Github:** Essential for version control and sharing my Python code and analysis.

# Data Preparation and Cleanup

#### This section outlines the necessary steps taken to prepare my data to ensure accuracy and usability.

## Importing and Cleaning the Data

I start by importing the necessary libraries and packages.  
These are:
- **Pandas**
- **Matplotlib.pyplot**
- **Seaborn**
- **ast** - Allows me to change a list surrounded by quotes back into a list.
- **load_dataset from datasets** - Allows me to import the data from Hugging Face

I then load the data using the load_dataset function from datasets and create a variable called df to store the data.

Finally I clean the data partially, specifically I change the job_posted_date column's data type from string to datetime. I do this so that manipulating it using built in functions is easier in the future. 

I also then change the job_skills column from the string data type to the list data type, again this makes it easier to manipulate in the future. I do this using the literal_eval function from the ast module, it essentially is used for container objects that are surrounded by quotes, or anything else that means python reads the data type incorrectly. Using the apply function in conjuction with this allows me to change the entire column in one line.

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

Creating 3 separate bar plots in one graph to answer this allows me to see the most demanded skills and how they compare between each of the top 3 job titles: Data Analyst, Data Engineer, and Data Scientist. I can then understand which skills are the most important for me to learn, dependant on the job role I'm interested in

#### Visualise Data

Here's the code I used to plot my final visualistion to answer this question:

```python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    job_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=job_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette= 'dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].set_xlim(0, 78)
    ax[i].legend().set_visible(False)

    for n, value in enumerate(job_plot['skill_percent']):
        ax[i].text(value + 1, n, f'{value:.0f}%', va='center')
    
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5);
```
#### Results

![Skill Demand Final Visualisation](3_Project\images\skills_demand_final_vis.png)

*Bar graph visualising the likelihood of job postings requesting the top 5 skills for 3 job titles.*

#### Insights:

- SQL is a highly demanded skill over all 3 roles that is clearly used a great deal, SQL has the highest demand for Data Engineer roles (68%) but is demanded in over half the jobs for Data Analysts and Data Scientists.

- Python is greately sought after for Data Scientist roles (72%) and is highly requested for Data Engineer roles (68%). As a versatile and impressive skill this is expected.

- From this I can see that Data Engineers require skills that aren't as widely used such as AWS, Azure, and Spark. Data Scientists seem to be requested for more high level skills as expected from the perfomance needed for the job and Data Analysts have a high demand for Excel (41%) which makes sense as the use of spreadsheets is very useful for the analysis of data.


## 2. How are in-demand skills trending for Data Analysts?

Investigating this question allows me to see whether certain skills were at risk of being less useful to learn. I created a line graph visualisation that showed me how likely it is for a skill to show up in a job posting over the year of 2023.

#### Visualise Data

Here's the code I used to plot my final visualistion to answer this question:

```python
df_plot = df_da_us_percent.iloc[:, :5]

sns.set_theme(style='ticks')
sns.lineplot(data=df_plot, dashes=False, palette='Greens_d')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

from adjustText import adjust_text

texts = []

for i in range(df_plot.shape[1]):
    col_name = df_plot.columns[i]
    y_val = df_plot.iloc[-1, i]
    
    if col_name.lower() == 'python':
        text = plt.text(11.4, y_val - 2, col_name) 
    else:
        text = plt.text(11.2, y_val, col_name)
    
    texts.append(text)

adjust_text(texts, arrowprops=dict())
plt.tight_layout()
```

#### Results
![Skills Trend Final Visualisation](3_Project\images\skills_trend_final_vis.png)

*Line graph visualising the trend of skills being demanded over the year of 2023.*

#### Insights:
- SQL seems to consistently be the most demanded skill for data analysts over the entire year of 2023, although it is declining slightly it is not doing so at an alarming rate.

- Excel comes in at the second spot with a steady start then a decline, however it does increase in demand at the end of the year.

- Tableau and Python come close together in demand throughout the year, however Tableau seems to stay just above Python. This could be because of the need for visualisation over complex analysis.

- Finally we have SAS that overall does not majorly increase or decrease in demand although there is some fluctuation.

- By looking at the end of the graph it looks to be that the demand for most of these skills starts to increase, collecting data on job postings for the year of 2024 would allow me to see if this trend is consistent or just a fluctuation. Overall I can see that SQL is the most demanded skill aswell as Excel, what I take away from this is that these two skills are the best to focus on.

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis of Data Roles
Creating boxplots to answer this question allows me to see major values for the each salary such as the median or lower and upper limits, plus outliers. Then from this I'm able to compare each salary and see how even the senior roles compare to entry level roles.

#### Visualise Data

Here's the code I used to plot one of my visualistions to answer this question:

```python
sns.set_theme(style='ticks')
sns.boxplot(data=df_us_top6, x='salary_year_avg', y='job_title_short', order=job_order, palette='Greens_d')

plt.title('Salary Distribution in the United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
```

#### Results
![Salary Distribution for the Top 6 Data Roles](3_Project\images\salary_analysis_vis1.png)

*A visualisation showing salary distribution for major data roles sorted by the median value.*

#### Insights
- By median salary I can see that Senior Data Scientists have the highest and Data Analysts have the lowest.

- An intersing insight I can take from this visualisation is that Data Scientists and Data Engineers have a higher median salary than the Senior Data Analyst role. This could mean after being a Data Analyst moving to one of those roles instead of becoming a Senior Data Analyst may be better for the increase in salary.

- Another thing we can look at is the outliers aswell as the whiskers of the boxplots, it seems that the Data Scientist roles have a wider spread of salary. Where as, based on the box and whiskers, the Senior Data Analyst role has a more centralised spread of salary.

- These insights could dictate the role I progress into and also the salary I can look to achieve.

### Highest Paid and Most Demanded Skills for Data Analysts

Looking at these factors allows me to understand the distribution of the salaries for Data Analysts, I can also understand what skills are the most valuable in the Data Analyst field.

#### Visualiise Data

Here's the code I used to plot one of my visualistions to answer this question:

```python
full_palette = sns.color_palette('Greens', 20)

top_colours = full_palette[10:]
bottom_colours = full_palette[:10]

fig, ax = plt.subplots(2, 1)

sns.set_theme(style="ticks")

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_da_top_pay, x='median', y=df_da_top_pay.index, ax=ax[0], hue='median', palette=top_colours)
ax[0].legend().remove()
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_da_skills, x='median', y=df_da_skills.index, ax=ax[1], hue='median', palette=bottom_colours)
ax[1].legend().remove()
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_ylabel('')
ax[1].set_xlim(ax[0].get_xlim()) # Set the same x-axis limit as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.tight_layout();
```
#### Results

![Highest Paid and Most Demanded Skills for Data Analysts](3_Project\images\salary_analysis_vis2.png)

*Two seperate graphs visualising the highest paid skills and the most in demmand skills for Data Analysts and their associated salaries in the US.*

#### Insights
- The top graph shows skills with very large salaries, however looking at the data I can see that the number of job postings per skill is very low. This signifies that the skills such as dplyr, Bitbucket and Gitlab are specialised skills and that being advanced skills means higher salaries even up to $200K.

- The bottom graph shows the most in demand skills and their associated salaries, although these are lower paying than the top graph they still have good median salaries. One thing we can look at is the difference between the top and bottom skills of this graph, on the lower end we have more Microsoft skills such as Powerpoint, Excel and Word compared to the more programming and visualisation skills at the top like Python, Tableau and R. What I gather from this is that it pays to have more advanced skills.

- Overall the disparity between the two graphs is clear, although taking into account both is helpful for gaining beneficial insights. I would say investing time into learning the high demand lower paying skills and also some of the low demand high paying skills can be beneficial. It can allow for someone to get into the data field and maybe earn more if their specialised skill was needed, however I would focus more on the foundational skills that are core to analytics first. 

## 4. What are the most optimal skills for Data Analysts to learn?

I created a scatter plot for this question and colour coded the points by the skill type, this allowed me to see the balance between a skills median salary and the percentage of job postings it was requested in. From here I could then see patterns within the skill categories.

#### Visualise Data

Here's the code I used to plot my final visualistion to answer this question:

```python
from adjustText import adjust_text

sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
texts = []

for i, txt in enumerate(df_da_skills_high_demand.index):
    x = df_da_skills_high_demand['skill_percent'].iloc[i]
    y = df_da_skills_high_demand['median_salary'].iloc[i]
    
    if txt.lower() == 'power bi':
        text = plt.text(x - 6.5, y - 500, txt)  
    elif txt.lower() == 'sas':
        text = plt.text(x - 0.5, y + 500, txt)  
    else:
        text = plt.text(x, y, txt)

    texts.append(text)

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.title('Most Optimal Skills for Data Analysts in the US')
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.tight_layout()
```

#### Results

![Most Optimal Skills for Data Analysts in the US](3_Project\images\optimal_skills_final_vis.png)

*A scatter plot visualising the most optimal skills (high paying & high demand) for Data Analysts in the US.*

#### Insights

- The scatter plot shows that the majority of the programming skills (coloured blue) tend to have a larger salary associated with them, their prevelance in job postings does seem to be spread from high to low. This signifies that these skills may offer higher salaries in the data analytics field. We can see that one of the programming skills, SQL, is the most prevelant among job postings, this tells me that it is a valuable and highly sought after skill. We can also see that Python has the highest median yearly salary and is not in a low amount of job posting's requests, this also tells me that it is a valuable skill.

- Analyst tools (coloured orange) seem to be spread out across the plot, we can see that Tableau, a visualisation tool, sits as the highest paid skill in this category with a large prevelance in job postings. However Excel is the most asked for skill at just under 50%, this signifies it is a worthwhile skill to learn. Power BI another visualisation tool sits high up on salary axis however its demand seems to be low at around 20%. Therefore these skills really have no major pattern in the data field and the demand and salary associated with them can fluctuate.

- Lastly we only have one skill for each of the last two categories, however one point we can make is that they are both in low demand for Data Analysts and therefore may not be a major priority for someone to learn who is looking to start at an entry level role. Another point to make is that they are both high paying, this signifies that maybe learning these later on in my career may be useful, again these seem to be valuable tools that analysts can use.

# What I learned

Throughout this project I've enhaced my Python skills as well as my problem solving skills, specifically in data manipulation and visualisation. I learned a lot about the data job market and gained insights specific to certain job titles, this not only helps me now but will help me for years to come as I transition between jobs.  
Here are a few specific things I learned:
- **The Importance of Data Cleaning:** I learned a lot about data cleaning but namely the importance of it, through practicing cleaning I could see how none of the questions I had, would have been answered correctly or even at all without cleaning and preparing the data. 
- **Advanced Python Usage:** I learned a lot through facing problems and having to use advanced Python concepts to solve them. Utilising libraries like Seaborn for complex visualisation helped me, I was able to create my goal and work towards it by researching and using trial and error.
- **Strategic Skill Analysis:** This project helped me to understand the importance of the links between skills, their demand, and salaries of job titles, this allows for a more in-depth planning for my career.
- **Creating Meaningful Goals:** Creating goals that hold importance to me personally, allowed for a more systematic analysis approach. I was able to make this project purposeful and I didn't feel as though I was just blindly writing code that didn't matter. I could therefore focus entirely on this project and really gain something from it.

# Insights

This project provided me with multiple general insights into the data job market, especially for analysts:

- **Skill Demand and Salary Correlation:** Although there is a correlation it is not direct. I can see that skills with very low demand, pay more. This could be because these skills are specialised and therefore are of high value if someone were to possess one. These are skills such as dplyr or Bitbucket. However we also see that the level of the skill also links to the salary associated with it. For example Python and R which have somewhat high demand pay more than Excel that has a higher demand than them, but then we see that SQL pays more than Excel and has a higher demand than it. The disparity between these seems to come down to the complexity associated with the skill rather than the demand.

- **Market Trends:** Looking at trends of the skill demand over the years for the top 5 skills allows me to understand which ones are worthwhile learning and where to place my time and effort. Without this I can't see if a skill seems to drop in demand over time and how it looks for the years to come, further analysis over the year of 2024 would extend my understanding, even using statistical analysis to create a predictive model for the next couple of years would be beneficial. Keeping up with these trends is crucial for career growth and stability.

- **Economic Value of Skills:** Researching which skills are highly in-demand and well-compensated can be advantageous for prioritising, again, my time for learning which skills and the level necessary to meet demands by employers. Having knowledge of the most optimal skills puts me at a vantage point to refine my skill set to be ideal for the job role I would like.

# Challenges I Faced

This project provided me with essential learning curves that allowed me to grow my knowledge and practice problem solving to gain my desired results.  
Here are some examples:

- **Data Inconsistencies:** When faced with missing or inconsistent data I had to make vital decisions between filing in the missing pieces or drop rows entirely based on the effect it would have on my visualisations and analysis. Making sure that these are dealt with is vital to ensure the integrity of my results.

- **Complex Data Visualisation:** Sometimes creating visualisations that portray the amount of information necessary to explain insights can be difficult with basic code. I had to research ways to fix problems like overlapping texts and axes formatting. This can be quite complex however I learned a lot of tricks that will help me in the future and that I can apply to a lot of my code to create cleaner and more appealing visualisations.

- **Balancing Depth and Width of Data:** Trying to decide how deep to dive into the data while also maintaining a wide view of the data overall can be challenging. I had to decide whether giving a wide yet general view of the data may be more beneficial, to gaining insights that have a larger impact, rather than a narrow but detailed view. A balance between the both can give a good understanding of multiple different aspects within the data, so aiming for this was ultimatley my goal throughout this project.

# Conclusion

This investigation into the data job market has been incredibly insightful, I have gained a lot of knowledge on the market around critical skills, expectations for salaries and how the proficiency of a skill may effect my results when job searching. As the market continues to change and grow, continuous analysis will be essential to stay updated and ahead in data analytics. This project is a great foundation for my future research and puts major emphasis on the importance of persistent learning in the data field.