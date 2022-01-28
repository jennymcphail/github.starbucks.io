<p align="left">
  <img width="460" height="300" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/starbucks.jfif?raw=true">
</p>

### Nano Degree Capstone Project
## How can Starbucks use data to inform their marketing strategy?

As my first "proper" job was in marketing, I was interested to see how campaign strategy could be influenced using data, using techniques I have learned during my Data Science Nano Degree.

This blog contains code snippets of interest, you can see my full code here: 

[capstone_starbucks](https://github.com/jennymcphail/capstone_starbucks)

My gut feel is that a combination of campaign attributes and customer attributes influences how successful a marketing strategy is, but does the data support this?

### About the Data (description supplied by Udacity)
This data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files:

**portfolio.json**
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

**profile.json**
* age (int) - age of the customer 
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

**transcript.json**
* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

## Data Engineering Part 1
The first interesting bit of data engineering I had to do was deal with a column which contained a dictionary. I split it out into separate columns and also used coalesce to combine two different spellings of the same thing.

<p align="left">
  <img width="1600" height="350" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/data_engineering1.JPG?raw=true">
</p>
