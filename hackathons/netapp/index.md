# NetApp

Brian McKean from NetApp presented to the class a data analysis problem he'd
like to get some help on.

# Overview

Listen to his presentation carefully. Write down the answers to the following
questions to show your team's understanding of the basics of the problem.

## What is the problem?
The system is supposed to be reporting in 24-hour increments, but it's not possible to tell whether the system is reporting correctly or not because some systems are not incrementing the base time, etc.

## Why is the problem important?
This is an issue because the company sends out people to perform fixes on the SSDs when they begin to wear out -- however, if the information is being reported incorrectly, ASUP may arrive at the wrong time and the cusomters may receive interruptions in service.

## What dataset has been made available?
A spreadsheet of that details the system, version, release, and times (base, observation, delta) of the system reports.

## What specific questions are being raised?
Due to the varied differences in delta time (when the systems are supposed to be reporting every 24 hours), it is important to be able to distinguish good data and bad data to determine which systems are reporting properly. In addition, it is impossible to know whether the base observation times are being incremented correctly, or that the time are being reported and reset every 24 hours.

# Q/A session

We will run a Q/A session. Before the session, compile a list of questions you
want to ask. Then, teams will take turn to ask Brian follow up questions.
Each team gets to ask one question each time. Write down the questions your team
wanted to ask and the answers you received. If another team happens to ask the
same question, simply write down the answer you heard.

## How is the data expected to be reported? Every day at a certain time, or just in random 24-hour increments?
The data should report all statistics from the previous 24-hour day.
The pull of statistics should reset base time and statistics in preparation for data from the next day.

## What is the threshold for what is considered "good" data vs. bad data?
Looking for recommendations, but most of the day. Ideally wouldn't miss periods of high usage. Both small and large delta times (observation time - base time) are bad. Note that there are no changes in behavior due to drive failure.

## What is the specific issue with the data in question?
The outlier delta times indicate some systems are reporting incorrectly -- as such, what patterns can be used? For instance, if a system is recording two days, then the data can simply be divided by two, while entries with very small delta times simply need to be thrown out.

## What information should be gathered after finding a good delta time?
Nothing -- what is needed right now is a good cutoff time that allows us to categorize the data.

## What time of day is the information coming from?
It's probably in the dataset somewhere -- we can determine based off of the times of recording whether a system reporting timeout matters. For instance, if the system doesn't report past midnight, it's likely that the issue isn't too significant because there isn't as much usage during those times.

## Where are the times coming from, and are they standardized?
Probably from the United States and Europe, and they are definitely not standardized.

# Approach

Based on the information you've obtained during the Q/A session, come up with 
plan for how your team will tackle this problem.

## How should the dataset be imported into Tableau?
As a .csv file.

## How should the work be distributed among the team members?
Team members can divide questions equally or group up and decided on a topic or issue to visualize.

## What types of charts or visualizations to use to support the answer?
Pie charts, bar chart, system delta time (or observation time) over time, box-and-whisker plots.

## What questions may be too complex for Tableau and may require custom scripts to be written?
Serial numbers may be an issue because there are so many different ones that it is difficult to generalize across them.

