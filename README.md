# FortunED
### The FortunED App provides analytics for prospective students, college students, and parents/guardians to project the Return on Investment (ROI) when considering majors, careers, and student loans for a college education.
<hr>

**Team:** Karl Ramsay, Swati Dontamsetti, Firzana Razak, Smiti Swain, Salvador Neves
<hr>

## Overview
As a team, some of use are parents, some of us are teachers, and all of us are continuing education students. We are passionate about education and the doors it can open for future career opportunities. But we understand that college can be expensive and that for some people it might not be the right route. We wanted to give a comprehensive look on the ROI of attending college for a chosen major. We approached this by first thinking about what each prong of our userbase would be interested in.

A high school student would be interested in:
1. The career options for a chosen major and the minimum degree required for each profession.
2. Specific majors under their major category along with the salary range over time and the unemployment rate.
3. What might be the most profitable state to work in for their major.
4. How much tuition they might have to pay for 4 years of college.

A college student would be interested in:
1. The career options for their major along with entry level salary range for each profession.
2. How long it would take to pay off their student loan.
3. What the living wage is for the state they are thinking about working in and whether there are better state's to work in for their major.
4. What the top 5 paying majors are for their chosen major/career category.

A parent/guardian would be interested in:
1. What the employment likelihood is for their child based on their gender and race in comparison to various levels of educational attainment.
2. How college tuition prices have changes for In-State and Out-of-State over time.
3. A way to access the high school or college student page to compare options for their child's future.

### Instructions
1. Open app/module folder in terminal or Git Bash.
2. Run **python manageData.py**. 
3. Open app folder.
4. Run **python app.py**. 
5. Open browser window and type http://127.0.0.1:5000/

### Some Considerations
- All of the data is based on state universities only. It was easy to find In-State and Out-of-State tuition prices over time.
- Since we only have one University per State, we are assuming that every University offers some version of our 13 major categories.

![approach.png](model/images/FortunEd-3-Stage_Approach.png)
<br>
![detailed-approach.png](model/images/FortunEd-Architecture.png)

## Ingest
We used Google Sheets to split up the work of finding datasets that would allow us to present our users with thorough information. We use a lot of education, employement, and career data from the <a href="https://www.bls.gov/emp/tables.htm">US Bureau of Labor Statistics</a> (BLS). Our university tuition data comes from the <a href="https://research.collegeboard.org/trends/college-pricing">CollegeBoard</a>. Our college majors dataset comes from <a href="https://www.kaggle.com/fivethirtyeight/fivethirtyeight-college-majors-dataset/data?select=majors-list.csv">FiveThirtyEight</a>'s Kaggle dataset. Our living wage data comes from <a href="https://livingwage.mit.edu/">MIT's Living Wage Calculator</a>, which also displayed median income per occupation that matched the BLS categories.

## Process
1. We used `Jupyter Notebook` to clean our datasets to just the data we are using.
2. We created mapping tables to link college majors to career categories.
3. We use `Pandas` to join the tables so that we have a link from major category to specific majors, and major category to occupation category to specific occupations.
4. We use `Sklearn` to create two different machine learning algorithms.
  <br>a. One is a classification that determines whether a chosen state is a good place to work based on student loan and living wage.
  <br>![classification.png](model/images/SVM_model_CR.PNG)
  <br>b. The other is a linear regression that extrapolated what university tuition will be for In-State and Out-of-State for the next two years.
  <br>![linear-regression.png](model/images/Logistic_Regression_CR.PNG)
5. We used `pymongo` and `MongoClient` to create dictionaries of all our records and then load it into `Mongo DB`.
6. We created `Python` functions to pull the specific data we need for specific charts and tables.
  <br>a. We split the work on `Zoom` and used `Slack` to log our discussion.
  <br>![work-split.png](model/images/slack_group_split.png)
  <br>b. We created specific sample `HTML` pages for each group member so we could each make and test our charts/tables without overriding each other's work when pushing to `Github`.
  <br>![sample-html.jpg](model/images/sample_html.jpg)
7. We then met up over `Zoom` to join all our `ChartJS` scripts and `Python` functions on their corresponding `HTML`, `functions.py`, and `app.py` sections.

## Digest
The final data was stored in a `Mongo` database, which was pulled from to obtain our various datasets for the charts and tables we want to display.

We used the micro-framework `Flask` inside of `Python` to create our website that would showcase our data. `Leaflet JS` and `Mapbox API` were used in `HTML` to create the map of our counties with the COVID case data used for coloring. Both the `Bootstrap`, and `ChartJS` libraries were used to beautify our website and create dynamic graphs.

![map.png](app/static/img/map.png)

![charts.png](app/static/img/charts.png)

## Final Results & Analysis
In general, our hypothesis is correct: as harder hit counties are more racially diverse and economically poor.

In the graphs presented here we'll compare Queens (the worst-hit county) with Hamilton (the least-hit county):

### Demographics
Queens is a lot **more** racially diverse compared to the state statistics.
![demoQ.png](app/static/img/demoQ.png)
Hamilton is a lot **less** racially diverse compared to the state statistics.
![demoH.png](app/static/img/demoH.png)

### Cases/Deaths & Poverty Levels
Queens had **60,236** COVID cases and is **above** the state poverty percentage.
![povQ.png](app/static/img/povQ.png)
Hamilton had **5** COVID cases and is well **below** the state poverty percentage.
![povH.png](app/static/img/povH.png)

### Median Income & Dow Jones Index
Queens is more racially diverse but the money is not spread out evenly. The white racial group tends to hold almost double the wealth of any other group, except for other (which is an ambiguous grouping with little info).
![incomeQ.png](app/static/img/incomeQ.png)
Hamilton doesn't even show data on the median income levels of the other racial groups in its county. But the white group makes about the same in Hamilton as it does in Queens, even though Queens is much poorer than Hamilton - which only makes the other racial groups in Queens that much poorer.
![incomeH.png](app/static/img/incomeH.png)
The Dow Jones Index drops in February as NY starts showing cases, it is at its lowest at end of March as we start to see the uptick in cases. But the Dow Jones Index almost immediately starts picking back up even though the COVID cases are still rising. It plateaus in April after the stimulus check is sent out on 4/9. COVID cases have seemingly (not confirmed) also platued but has not decreased yet. The stimulus may have played an effect in stabilizing the Dow Jones Index, but so too might the global control of COVID cases. There are many variables we weren't able to consider for this graph. All we know is that COVID is still on the rises, and yet the Dow Jones Index is picking up.
