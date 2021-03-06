1)	We pulled 3 csv files from Kaggle.com (competitions.csv, teams.csv, and results.csv). 

2)	Using Jupyter Notebook to clean, filter, join, and aggregate data covering BBQ competitions held in Kansas City, MO, we were able to merge three distinct csv files into one cohesive data set. The first issue was figuring out how the three data sets related to one another. Once we had our keys to the data, we were able to merge the sets together. 
First, we had to manipulate the columns in order to get rid of duplicates. Then we had to join the teams data with the results data through the team_id key. Once we had our joined data, we were able to bring in the competition data using competition_id merged with competition_id in our previous joined data set. 
After we got all our data adjusted and joined, we were able to clean the data set by establishing which columns were necessary and which structure we wanted. From there we simply staged the data in pgAdmin. 
3)	We placed the final data in PostgreSQL under the name Kansas_City_BBQ.

4)	The data collected includes all of the results from barbeques in Kansas City, MO, the BBQ competitions that took place in Kansas City, MO, all of the teams that participated in those competitions in Kansas City, MO, and the results from those competitions in Kansas City, MO.

 

Postgres Export
-- Exported from QuickDBD: https://www.quickdatabasediagrams.com/
-- Link to schema: https://app.quickdatabasediagrams.com/#/d/t4aEDe
-- NOTE! If you have used non-SQL datatypes in your design, you will have to change these here.


CREATE TABLE "teams" (
    "id" serial   NOT NULL,
    "name" varchar(75)   NOT NULL,
    CONSTRAINT "pk_teams" PRIMARY KEY (
        "id"
     )
);

CREATE TABLE "competitions_df" (
    "id" serial   NOT NULL,
    "kcbs_competition_id" int   NOT NULL,
    "location" varchar(100)   NOT NULL,
    "start_date" date   NOT NULL,
    CONSTRAINT "pk_competitions_df" PRIMARY KEY (
        "kcbs_competition_id"
     )
);

CREATE TABLE "results" (
    "id" serial   NOT NULL,
    "competition_id" int   NOT NULL,
    "team_id" int   NOT NULL,
    "place_rank" int   NOT NULL,
    "category" varchar(50)   NOT NULL,
    CONSTRAINT "pk_results" PRIMARY KEY (
        "id"
     )
);

ALTER TABLE "teams" ADD CONSTRAINT "fk_teams_id" FOREIGN KEY("id")
REFERENCES "results" ("team_id");

ALTER TABLE "results" ADD CONSTRAINT "fk_results_competition_id" FOREIGN KEY("competition_id")
REFERENCES "competitions_df" ("kcbs_competition_id");

select teams.*, results.*, kc_bbq.*
from kc_bbq inner join teams on kc_bbq.id = teams.id
inner join results on kc_bbq.kcbs_competition_id = results.competition_id



select teams.id, teams.name, results.id, results.competition_id, results.team_id, results.place_rank, results.category,  kc_bbq.id, kc_bbq.kcbs_competition_id, kc_bbq.location
from kc_bbq inner join teams on kc_bbq.id = teams.id
inner join results on kc_bbq.kcbs_competition_id = results.id

select teams.name, 
teams.id,
results.place_rank,
results.category,
kc_bbq.location,
kc_bbq.start_date
from kc_bbq inner join teams on kc_bbq.id = teams.id
inner join results on kc_bbq.kcbs_competition_id = results.competition_id



# Guidelines for ETL Project

This document contains guidelines, requirements, and suggestions for Project 1.

## Team Effort

Due to the short timeline, teamwork will be crucial to the success of this project! Work closely with your team through all phases of the project to ensure that there are no surprises at the end of the week.

Working in a group enables you to tackle more difficult problems than you'd be able to working alone. In other words, working in a group allows you to **work smart** and **dream big**. Take advantage of it!

## Project Proposal

Before you start writing any code, remember that you only have one week to complete this project. View this project as a typical assignment from work. Imagine a bunch of data came in and you and your team are tasked with migrating it to a production data base.

Take advantage of your Instructor and TA support during office hours and class project work time. They are a valuable resource and can help you stay on track.

## Finding Data

Your project must use 2 or more sources of data. We recommend the following sites to use as sources of data:

* [data.world](https://data.world/)

* [Kaggle](https://www.kaggle.com/)

You can also use APIs or data scraped from the web. However, get approval from your instructor first. Again, there is only a week to complete this!

## Data Cleanup & Analysis

Once you have identified your datasets, perform ETL on the data. Make sure to plan and document the following:

* The sources of data that you will extract from.

* The type of transformation needed for this data (cleaning, joining, filtering, aggregating, etc).

* The type of final production database to load the data into (relational or non-relational).

* The final tables or collections that will be used in the production database.

You will be required to submit a final technical report with the above information and steps required to reproduce your ETL process.

## Project Report

At the end of the week, your team will submit a Final Report that describes the following:

* **E**xtract: your original data sources and how the data was formatted (CSV, JSON, pgAdmin 4, etc).

* **T**ransform: what data cleaning or transformation was required.

* **L**oad: the final database, tables/collections, and why this was chosen.

Please upload the report to Github and submit a link to Bootcampspot.

- - -

### Copyright

?? 2021 Trilogy Education Services, LLC, a 2U, Inc. brand. Confidential and Proprietary. All Rights Reserved.
