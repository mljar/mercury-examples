---
title: [BEST TITLE]
categories:
  - Mercury Tutorial
date: 2023-04-20
author: Adrian Błazeusz
comments: true
---
!![enter image description here][14]

Data analyst is becoming an increasingly desirable profession, on the side of the employeeas well as the company. No wonder companies need specialists to help them make important business decisions. However, the market situation is becoming difficult, especially for people whose goal is to getting their first job. In this tutorial, we will show you two useful things for you, namely:How to create a website using Mercury. And at the same time we will analyze the job market to show you what skills will help you get your first job more easily! 

# How to create a website?
It's time to open Jupiter Notebook! After installing Mercury, we are only a few lines of code away from creating a website.
In the first cell we will import mercury and other important library to your notebook.
```
pip install mercury
or
conda install -c conda-forge mercury
```

```
import mercury as mr
```

![enter image description here][1]

## Mercury App
In the second cell we will configure the Mercury App you will choose title and description: 
Try to describe your project originally, it will help later to locate your work.
```
app = mr.App(title="Analysis for analysts",description="Data analysis relating to the skills a Data Analyst needs.", show_code=show_code.value)
```

![enter image description here][2]

Mercury after starting the second cell will pop you a notification , "Mercury Application This output won't appear in the web app."
Your notebook can officially be a page now. But before we launch it, let's add some widgets. 

## MultipleSelect for experience

In this scenario, we want to add MultiSelect for the job expertise as well as for your skills. Let's define our first multiselect as well as specify our options to choose from.
```
lvl = mr.MultiSelect(value=['Junior'],choices=['Junior','Mid','Senior'], label="Choose youre level")
```
![enter image description here][3]

The task of the next cell is to select from our DataFrame the levels of advancement we have chosen.
```
df_lvl = df[df.category.isin(lvl.value)]
```
![enter image description here][4]

And just create a graph using matplotlib. 
```
level = df_lvl['category']
level.plot(kind='bar')
```
![enter image description here][5]

From now on, our chart will show only what we have marked.

## MultipleSelect for skills

Let's add another MultiSelect to our code.
```
sk_set = mr.MultiSelect(choices=['sql', 'python','snowflake'...], 
label="Choose your skills", value=['sql', 'python', 'r', 'snowflake'])
```
```
sk_df = skills_df[skills_df.skill.isin(sk_set.value)]
```
```
fig, ax = plt.subplots()
ax.bar(sk_df['skill'], sk_df['avg_salary'], color='red', alpha=0.8)
```
![enter image description here][6]

And all done!

## Mercury run
Mercury will read all your code and create a web app. Now open your the command line and navigate to the location of your notebook. Once you've set everything up just type mercury run in the terminal.

![enter image description here][7]

The application should open the page by itself, but if it doesn't, you can copy the html: from your ternimal.

![enter image description here][8]

If everything has been done correctly you should now see this screen and your file.
When you click on it, your own page will open.

# Market analysis
As you can see, Mercury gives time-savers and changes ordinary notebooks to a more user-friendly as well as adding the ability to interact with data. Let's move on to analyzing the market situation.

## Who is looking for the largest number of analysts?
The freelance company Upwork with more than 2,000 job offerts is dramatically breaking through.

![enter image description here][9]

## Where can you find the most job offerts?
Linkedin is positioned first, and right after that we see Upwork again.

![enter image description here][10]

## What types of jobs are there?
Of course, most companies need full-time analysts.And when it comes to the possibility of working 
remotely there are more than 5,000 listings with this option.

![enter image description here][11]

## What skills are most in demand? 
According to the chart, the most sought-after skills are SQL, Excel and Python.
But, the best-paid skills are Snowflake, Python, R and in fourth place is SQL.

![enter image description here][12]

![enter image description here][13]

# Summary
Thanks to mercury we ca build a website in two line of code, and add widgets. 


  [1]:
  [2]: 
  [3]: 
  [4]: 
  [5]: 
  [6]: 
  [7]: 
  [8]: 
  [9]:
  [10]:
  [11]:
  [12]:
  [14]:
  [15]: