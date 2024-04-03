---
layout: post
title:  "Exploring Data Science Job Market: Unveiling Trends through Glassdoor Scraping"
author: Meagan Bolton
description: "In today's digital era, data science stands at the forefront of innovation, driving decisions and shaping industries. 
In this blog post, I share my journey of curating a comprehensive dataset on data science job postings using web scraping 
techniques and delve into the process of extracting valuable insights."
image: "/assets/images/introPicture.jpg"
---

## Introduction
As I've started to apply for jobs in the data science world I find myself giddy looking at the job listings. So many options 
and opportunities not to mention the massive pay increase from my current on-campus job. This surge of excitement has spurred 
me to ponder various questions: What salary range should I anticipate?
Which companies and locations boast the most enticing offers? Is there a correlation between the ratings of companies and the 
salaries they offer? In pursuit of answers, I embarked on a journey to scrape job listings data from Glassdoor.

## Step 1: Initialize Chrome WebDriver
Before we begin, make sure you have the following installed:

* Python 3.x
* Selenium library (pip install selenium)
* Chrome WebDriver
* ChromeDriverManager (pip install webdriver-manager)

First, let's initialize the Chrome WebDriver using the ChromeDriverManager to ensure compatibility.

```{python}
import time
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(ChromeDriverManager().install())
```

## Step 2: Log into Glassdoor
For the method I used you will need to have a Glassdoor account. You could create one through selenium but it would be 
far less work to log onto Glassdoor and create an account before we begin trying to scrape the data.

Now we are ready to really start coding!
1. You want to get the URL to the GlassDoor login page.
```{python}
# Load Glassdoor login page
driver.get('https://www.glassdoor.com/profile/login_input.htm')
```
2. On the first page it only requires you to enter your email and click the next button. (Note: the generic email "youremail@gmail.com" is used but
you'll want to replace that with your own email)
```{python}
# Enter email
email_input_field = driver.find_element_by_class_name('email-input').find_element_by_tag_name('input')
email_input_field.send_keys(youremail@gmail.com)

# Click 'Continue with Email' button
continue_with_email_button = driver.find_element_by_class_name('emailButton')
continue_with_email_button.click()
```
3. On the next page you will be required to enter in your password to move forwards. (Again a generic password is used and will
   need to be replaced)
```{python}
# Enter password
password_input = driver.find_element_by_id('inlineUserPassword')
password_input.send_keys(your_password)

# Click 'Sign in' button
continue_with_signin_button = driver.find_element_by_class_name('emailButton')
continue_with_signin_button.click()
```
4. Making your email and password Github ready. If you plan don't plan on posting your code to GitHub you are free to skip this
   step, however, if you do want to post this and don't want to give out your Glassdoor login information this step is for you.
   - Save your email and password in text files. Mine are called email.txt and password.txt.
   - You can then use this code to read in the email and password without releasing any of your information:
     ```{python}
     # Getting the email and password I will need to login
     with open('email.txt', 'r') as f:
       email_login = f.read().strip()
     with open('password.txt', 'r') as f:
       password_login = f.read().strip()
     ```

## Step 3: Navigate to Job Search Page
Next, we'll navigate to the job search page and specify the search criteria, such as job title and location.

You can use this URL to navigate to the job search page.
```{python}
# Navigate to the job search page
driver.get('https://www.glassdoor.com/Job/jobs.htm')
```
Once you are at the job search page you will want to put in the criteria for what you're looking for.
In the example below 'data science' is used for the job title section of the search bar and 'New York, NY' is used for
the location section.
```{python}
# Enter job search criteria
search_input = driver.find_element_by_id('searchBar-jobTitle')
search_input.send_keys('data science')

location_input = driver.find_element_by_id('searchBar-location')
location_input.send_keys('New York, NY')
```
## Step 4: Scrape Job Listings
Once you get to the page with the job listings you are interested in it's time to scrape!

Below is an example of how you might get the salaries of listed jobs. (It's important to note that not all the information
will be listed on each job listing, so watch out for that!)
```{python}
jobs = driver.find_elements_by_css_selector('[data-test="jobListing"]')

#### loop through job posting and scrape data
for job in jobs:
  try:
    # Extracting the salary if it is listed
    salary = job.find_element_by_css_selector('div.JobCard_salaryEstimate__arV5J').text
  except:
    salary = 'NA'  
```

There is a lot of information that you can find on Glassdoor so feel free to take as much as you'd like. Keep in mind that
some of this information might not be quite ready to be used.

![Alt Text](/assets/images/NY data screenshot.png)
## Crafting the Dataset
Data Source: Leveraging web scraping techniques, I collected job postings from Glassdoor, a leading platform for career 
insights and employer reviews. By targeting key locations such as New York, NY; Houston, TX; San Francisco, CA; Seattle, WA; 
Chicago, IL; and Raleigh, NC, I aimed to capture regional variations in data science job offerings.

Data Collection Process: Using Python's Selenium library, I automated the process of navigating through Glassdoor's job 
search interface, systematically retrieving job titles, company names, locations, salaries, and employer ratings. Each 
job posting served as a valuable data point, contributing to the richness of the dataset.

## Preparing the Dataset
Cleaning and Augmentation: Upon gathering the raw data, I employed various data preprocessing techniques to enhance its 
quality and usability. This involved tasks such as standardizing salary formats, identifying pay rates (hourly vs. yearly), 
and extracting pertinent information while eliminating extraneous details such as "(Employer est.)" and "(Glassdoor est.)".

Feature Engineering: To enrich the dataset further, I introduced a new feature, "Pay rate", which categorizes salaries based 
on their frequency (hourly or yearly). This transformation not only facilitates comparative analysis but also offers 
valuable insights into prevailing compensation structures across different job markets.

## Exploratory Data Analysis (EDA)
Unveiling Patterns: Armed with a refined dataset, I delved into exploratory data analysis to uncover underlying patterns and 
trends within the data science job market. Through visualizations and statistical summaries, I elucidated factors such as 
salary distributions, regional variations, and the relationship between employer ratings and compensation.

Key Findings: The EDA revealed intriguing insights, including disparities in salary ranges across cities, the prevalence of 
hourly vs. yearly pay rates, and correlations between employer ratings and offered salaries. These findings serve as a 
testament to the multifaceted nature of the data science job landscape and offer valuable guidance to job seekers and 
employers alike.

## Conclusion
In this blog post, I've provided a glimpse into the process of curating and analyzing a comprehensive dataset on data science 
job postings. By leveraging web scraping techniques and employing data preprocessing methods, I've uncovered valuable insights 
into the evolving dynamics of the job market. Moving forward, I'm excited to delve deeper into the data, unraveling more 
nuanced trends and fostering a deeper understanding of the intricate interplay between talent acquisition and industry demands.

For access to the complete dataset and code repository, please visit GitHub. If you have any questions, insights, or 
suggestions for future analyses, feel free to share them in the comments below!
