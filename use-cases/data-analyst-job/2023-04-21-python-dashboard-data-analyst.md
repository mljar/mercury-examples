---
date: 2023-04-21
title: Python Dashboard for 15,963 Data Analyst job listings
categories:
  - Matplotlib
  - Jupyter
author: Adrian Błazeusz
comments: false
image: /blog/python-dashboard-data-analyst/banner.jpg
---

<img src="/blog/python-dashboard-data-analyst/banner.jpg" alt="Interactive Matplotlib plots in Jupyter Notebook"/>There are tons of job openings in the U.S.A job market and even more different requirements needed to get your first job as a Data Analyst, so why not take this as another challenge to analyze? In this article, we'll use data from the Internet to create a project for a portfolio while pulling out the most useful information.It will be EDA based on data analyst job offers.

## About data

We will be using a dataset from kaggle.com [link here](https://www.kaggle.com/datasets/lukebarousse/data-analyst-job-postings-google-search?datasetId=2614070&sortBy=voteCount).
If you want to check out my **full code**, you can find it here [link](https://github.com/mljar/mercury-examples/tree/main/use-cases/data-analyst-job).

This dataset pulls job postings from Google's search results for Data Analyst positions in the United States, and pulling evry usful information such us: **tytle, company, location, description and via**.

As is often the case with this type of data scraping method, some data like to hide. Let's take a closer look at what we will have to deal with.

- The dataset has 15,963 job offers. 
- 80% of the payout data is missing. Understandably, most companies will want to negotiate the price. 

## Import

It's time to open Jupyter Notebook, and **install important library** (if you haven't done this already).

```
pip install mercury
or
conda install -c conda-forge mercury
 
pip install wordcloud
 
pip install numpy
 
pip install pandas

pip install matplotlib
```

In the first cell, we will **import mercury** and other important library for this project.

```python
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import mercury as mr
from wordcloud import WordCloud

import re
from collections import Counter
from dateutil.parser import parse
from string import punctuation as punct
```
- **numpy** provides support for numerical operations.
- **pandas** is a library for data manipulation and analysis.
- **matplotlib.pyplot** allows you to visualize data and create charts.
- **mercury** allows you to create pages from notepad and add widgets.
- **wordclud** allows the creation of word clouds.
- **re** allows you to perform operations on regular expressions. 
- **collections** contains container types specialized for storing objects. 
- **dateutil.parser** provides tools for parsing and manipulating dates and times. 
- **string** contains a set of constants, such as punctuation marks, that can be used in operations on text.

## Data preprocessing

- We will create a new column **category** in which we will have the level of experience (Junior, Mid, Senior). Based on it, we will create charts for the levels.
- From the descriptions, we will take information regarding the **skills** that are required in each job offer. When we will have ready a column with skills as well as **salaries based** on them, we will draw conclusions which skills are more profitable.
- We will fill in the missing salary values.

<img src="/blog/python-dashboard-data-analyst/preprocessing.png" class="blog_img_100 blog_img_border" alt="Data preprocessing."/> 

### Job experience

The first step will be to **create a set of keywords** for levels of experience as well as skill.

```python
junior_keywords = {'junior', 'entry level', 'intern', 'apprentice','jr','I'}
mid_keywords = {'mid-level', 'mid level', 'midlevel','mid', 'II','consultant', 'intermediate', 'experienced','associate','practitioner'}
senior_keywords = {'senior', 'lead', 'principal', 'sr', 'III', 'IV', 'senior level','advanced','senior associate', 'expert'}
```

Let's create a function that will assign columnists to a given division.

```python
# function to assign an offer to a category
def categorize_job(row):
    title = row['title'].lower()
    description = row['description'].lower()
    for keyword in junior_keywords:
        if keyword in title or keyword in description:
            return 'Junior'
    for keyword in mid_keywords:
        if keyword in title or keyword in description:
.            return 'Mid'
    for keyword in senior_keywords:
        if keyword in title or keyword in description:
            return 'Senior'
    return 'unknown'

# add column 
df['category'] = df.apply(categorize_job, axis=1)
```

The function **first converts the title and description** of the job to lowercase using the **lower()** method to ensure that all keywords are matched regardless of case. **Next, the function iterates over each keyword** in the junior, mid and senior keywords lists. If a keyword is found in either the title or the description, the function **returns the corresponding category** (Junior, Mid, or Senior). **If none of the keywords are found, the function returns 'unknown'.**

Now that we have assigned a category to each job listing, we can further clean the data by removing job listings that belong to the "unknown" category.

```python
df = df.drop(df[df['category'] == 'unknown'].index)
```

### Skills & missing salary

It's time for finding skills in offers.
Let's **combine our keywords for skills**, and drop data with null description.

```python
keywords_programming = {'sql', 'python', ...}
keywords_libraries = {'scikit-learn', 'jupyter', ...}
keywords_analyst_tools = {'excel', 'tableau', ...}

keywords = keywords_programming.union(keywords_libraries).union(keywords_analyst_tools)

#drop rows with null descriptions
jobs_all = df.dropna(subset=['description']).reset_index()
```

```python
from collections import Counter

keyword_counts = Counter()
for description in jobs_all['description']:
    words = description.lower().split()
    keyword_counts.update(set(words).intersection(keywords))
```

The code initializes a **Counter** object to **count the number of occurrences** of each keyword in a set of job descriptions. It then **iterates through each description**, converts it to lowercase, splits it into individual words, and **updates the Counter** object with the number of occurrences of each keyword in that description.

```python
# sort the keywords by frequency in descending order
sorted_counts = keyword_counts.most_common()
```

```python
# extract the 10 most common keywords
top_keywords = sorted_counts[:10]
```

We will use the above code **top_keywords** to create a top 10 skills chart for a data analyst afterwards.

```python
skills = []

for val in df.description_tokens.values:
    skills.extend(val)
skills, counts = np.unique(skills, return_counts=True)
skill_count = sorted(zip(list(skills), list(counts)), key=lambda x: -x[1])
skills = list(map(lambda x: x[0], skill_count))
counts = list(map(lambda x: x[1], skill_count))
salaries = []
for skill in skills: 
    salaries.append(df[df.description_tokens.apply(lambda x: skill in x)].salary_standardized.fillna(0).mean())
skills_df = pd.DataFrame({"skill": list(skills), "number_of_postings": counts, "avg_salary": map(round, salaries)})
skills_df = skills_df.sort_values("avg_salary", ascending=False)

```

**First**, the code creates an **empty "skills" list**, and then adds to it all the **words appearing in job descriptions**. It then counts how many times each word appeared in the text, sorts the list in descending order of the number of occurrences, and **creates two lists**, **"skills" and "counts"** storing information about the words and their frequency.**And finally**, the code **calculates the average salary** for each of the words in the "skills" list and stores them in the "salaries" list.

```python
skills_salary = []
for skill in skills:
    skill_salary = df[df.description_tokens.apply(lambda x: skill in x)].salary_yearly.mean()
    skills_salary.append(skill_salary)
skills_df["avg_salary_yearly"] = skills_salary
```

For each word, the code creates a filter that **selects the rows** in the DataFrame for which the **word appears in the job description** (in the 'description_tokens' column), and then **calculates the average annual salary** for these rows. Finally, the code adds a new column "avg_salary_yearly" to the DataFrame "skills_df", containing the calculated average annual salaries for each word in the "skills" list.


## Adding widgets and create charts

We are only a few lines of code away from creating a website. But to have something to show, we need to add couple functions from Mercury to make our interactive charts and information.

### Show or hide code switch

The task of this cell is to determine the **checkbox**, so that we can decide with one click whether we want to show our code on the page.

```python
show_code_switch = mr.Checkbox(label="Show code", value=False)
```

## Mercury App

To create a fully functioning site, we need to specify a feature called Mercury App. 
Try to describe your project originally, it will help you later to locate your work.

```
app = mr.App(title="Analysis for analysts",description="Data analysis relating to
 the skills a Data Analyst needs.", show_code=show_code_switch.value, continuous_update=False)
```

Mercury after running this cell will pop you a notification, "Mercury Application This output won't appear in the web app."
**Your notebook can officially be a page now**. But before we launch it, let's add some widgets and create charts. 

### MultiSelect for experience

In this scenario, we want to **add MultiSelect** for the job expertise as well as for your skills. Let's define our first MultiSelect as well as **specify our options to choose from**.

```python
lvl = mr.MultiSelect(value=['Junior','Mid','Senior'],
choices=['Junior','Mid','Senior'], label="Choose youre level")
```

The task of the next cell is to **select from our DataFrame the levels** of advancement we have chosen.

```python
df_lvl = df[df.category.isin(lvl.value)]
```

And just create a graph using **matplotlib**. 

```python
#defind size and style
plt.figure(figsize=(12,6))
#add only to first plot
plt.style.use('ggplot')

#create plot
level = df_lvl['category'].value_counts()
level.plot(kind='bar')

#add text
plt.title(mr.Md(f"# Number of jobs for {', '.join(lvl.value)}"))
plt.ylabel("Counts")
plt.xticks(rotation=45)

plt.show()
```

The title is formatted using the f-string format and the **join() method to concatenate** the values in the **lvl variable**.
From now on, our chart will show only what we have marked and only the title for ouer specyfic levels.

**The next three charts** are done exactly the same way, just change the column name in the **df_lvl[ ]** to something else, for example:
- 'company_name'
- 'via'
- 'schedule_type'

### Remote work

```python
dummies = pd.get_dummies(df_lvl['work_from_home'], prefix='remote', prefix_sep='_')
```

To make a chart with remote work, I chose the **get_dummies** function from the pandas library. The **pd.get_dummies()** function **creates new columns** with the prefix 'remote_' and binary values for each unique value from the 'work_from_home' column.

```python
plt.figure(figsize=(12,6))
counts = dummies.value_counts()
yn = ["You can't work remotley", 'You can work remotely']
plt.bar(yn, counts)
```

The task of **yn is to change the names** of the straw charts to my choice.

### Creating Markdown

If you want to add more specyfic information about data you can use **Markdown**, with this you can change value for your text.

```python
top_3_companies = df_lvl['company_name'].value_counts().nlargest(3).index.tolist()

top_contract = df_lvl['schedule_type'].value_counts().nlargest(1).index.tolist()

top_3_web = df_lvl['via'].value_counts().nlargest(3).index.tolist()
```

The first line of code **top_3_companies** counts the frequency of values in the column 'company_name' of the df_lvl, **selects the top three most frequently occurring** values, and **converts them to a list.** 

```python
remote_jobs_percent = round((df_lvl['work_from_home'].sum() / len(df_lvl)) * 100, 1)
```

The fourth line of code **remote_jobs_percent** calculates the percentage of rows in the df_lvl that have the value 'True' in the 'work_from_home' column, **rounds the**
**result to one decimal place**, and assigns the value to the variable **remote_jobs_percent.**

To create text that will use our variable data we will use a function in mercury called **Markdown.** 

<img src="/blog/python-dashboard-data-analyst/text.png" class="blog_img_100 blog_img_border" alt="Markdown for about data"/> 

### Word Cloud

To make our presentation more interesting we can add **Word Cloud.** This is a **collection of all the sub-counted words,** the size of the word depends on the number of times the word is repeated in the text.

```python
# sort the keywords by frequency in descending order
sorted_counts = keyword_counts.most_common()

# create a dictionary from the sorted_counts variable
word_dict = dict(sorted_counts)

# generate the word cloud
wordcloud = WordCloud(width = 800, height = 800, 
                background_color ='white', 
                min_font_size = 10).generate_from_frequencies(word_dict)
```

<img src="/blog/python-dashboard-data-analyst/wordcloud.png" class="blog_img_100 blog_img_border" alt="Word cloud for data analyst word."/> 

### MultiSelect for skills

Let's add another **MultiSelect** to our code. What's good about this module is that you can add **as many options as you want.**

```python
sk_set = mr.MultiSelect(choices=['sql', 'python','snowflake'...], 
label="Choose your skills", value=['sql', 'python'...])
```

```python
sk_df = skills_df[skills_df.skill.isin(sk_set.value)]
```

The second cell just like **df_lvl** creates us a dataframe that is **customized to our selected options** in MultiSelect. 

```python
fig, ax = plt.subplots(figsize=(12,6))
ax.bar(sk_df['skill'], sk_df['avg_salary']*12, color='red', alpha=0.8)
ax.set_xlabel('Skills')
ax.set_ylabel('Avg. salary yearly')
ax.set_title('Average salaries by skills')
plt.xticks(rotation=45)

# Set y-axis ticks and labels
max_salary = sk_df['avg_salary'].max() * 12
yticks = np.linspace(0, max_salary, 10)
yticklabels = ['$' + '{:,.0f}'.format(y) for y in yticks]
ax.set_yticks(yticks)
ax.set_yticklabels(yticklabels)
plt.show()
```
Second part of the code sets the **y-axis** ticks and labels using the maximum salary value, max_salary, and the **np.linspace()** function, which **creates a linearly spaced** array of values. The **yticklabels** are formatted as dollar values with comma separators using the **'{:,.0f}'.format()** string formatting method.

And all done!

### Mercury run

Mercury will read all your code and **create a web app**. Now open your the command line and navigate to the location of your notebook. Once you've set everything up just type **mercury run** in the terminal.

<img src="/blog/python-dashboard-data-analyst/cd-mercury.png" class="blog_img_100 blog_img_border" alt="Mercury run."/> 

The application should open the page by itself, but if it doesn't, you can copy the html: from your ternimal.

<img src="/blog/python-dashboard-data-analyst/web-menu.png" class="blog_img_100 blog_img_border" alt="Mercury web menu."/> 

If everything has been done correctly, you should now see this screen and your file.
When you click on it, your own page will open.

<img src="/blog/python-dashboard-data-analyst/web.png" class="blog_img_100 blog_img_border" alt="Mercury Web app."/> 

## Market analysis

As you can see, Mercury gives time-savers and changes ordinary notebooks to a more user-friendly as well as adding interact with data. Let's move on to analyzing the market situation.

### Who is looking for the largest number of analysts?

The freelance company **Upwork** with more than 2,000 job offerts is dramatically breaking through.

<img src="/blog/python-dashboard-data-analyst/what-company.png" class="blog_img_100 blog_img_border" alt="Who is looking for the largest number of analysts?"/> 

### Where can you find the most job offers?

**Linkedin** is positioned first, and right after that we see Upwork again.

<img src="/blog/python-dashboard-data-analyst/where-to-look.png" class="blog_img_100 blog_img_border" alt="Where can you find the most job offers?"/> 

### What are the types of conctract?

Of course, most companies need **full-time analysts**.

<img src="/blog/python-dashboard-data-analyst/contract-type.png" class="blog_img_100 blog_img_border" alt="What are the types of conctract?"/> 

And when it comes to the possibility of **working remotely** there are more than **5,000** listings with this option.

<img src="/blog/python-dashboard-data-analyst/remote.png" class="blog_img_100 blog_img_border" alt="How many offers?"/> 

### What skills are most in demand? 

According to the chart, the most sought-after skills are **SQL, Excel and Python.**

<img src="/blog/python-dashboard-data-analyst/10-skills.png" class="blog_img_100 blog_img_border" alt="What skills are most in demand? "/> 

But companies will be more willing to pay you more if you will known: **Jupyter, Tableaut, Snowflake and SQL**.

<img src="/blog/python-dashboard-data-analyst/avg-pay-skills.png" class="blog_img_100 blog_img_border" alt="Most pay skills."/> 

## Summary

In this project, we went through preparing a given, creating charts and making website. We have analyzed the American labor market and extracted the most important information regarding skills, disbursement of contract types as well as experience needed for the job. And after this we created a website with widgets for easier presentation of data using mercury.