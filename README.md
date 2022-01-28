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
Now I am able to look at metrics like offers per person and how many people received each offer.

## Analysis Visualisation Part 1
Using mean told me that on average each person received 3.7 offers. But of course you can't receive 0.7 of an offer, so I also made a pie chart to visualise how many people received 1,2,3,4 etc. offers. I think this looks good as a pie chart showing the percentage of people falling into each group and I also like a bit of white space to separate each slice of the pie!
<p align="left">
  <img width="1600" height="200" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation1.JPG?raw=true">
</p>
<p align="center">
  <img width="600" height="600" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation1a.jpg?raw=true">
</p>
Over two-thirds of people received 3 or 4 offers and the highest number received was 6. I also ran counts which showed roughly similar numbers of people received each offer (around 6,300).

## Data Engineering Part 2
What are the potential journeys that each customer could have when they receive an offer?
- received, viewed, completed
- received, viewed, not completed
- received, not viewed, completed
- received, not viewed, not completed

I turned my transcript dataframe into a journey one which maps these and counts how many customers have done each journey type (where 111 = Received, Viewed, Completed, 110 = Received, Viewed, Not Completed etc.)
<p align="left">
  <img width="1600" height="450" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/data_engineering2.jpg?raw=true">
</p>

But I think you'll agree, this top level summary isn't very useful or informative.

## Analysis Visualisation Part 2 (with a bit of iteration)
I wrote an iterative process to create multiple stacked bar charts grouped by offer and also by each feature of an offer (e.g. channel, type, reward etc.)
This snippet just shows the iterative chart creation, see my full code for the much longer code I used to prepare the data first.
<p align="left">
  <img width="800" height="200" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2.jpg?raw=true">
</p>

<img width="1604" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2a.jpg?raw=true"> 
<img width="1604"  src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2b.jpg?raw=true">
<img width="1604" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2c.jpg?raw=true">
<img width="1604" src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2d.jpg?raw=true">
<img width="1604"  src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2e.jpg?raw=true">
<img width="1604"  src="https://github.com/jennymcphail/github.starbucks.io/blob/main/visualisation2f.jpg?raw=true">

From this we can see the following:
- Each offer has a different journey profile - for example the fourth and sixth offers have not been completed at all
- If you look at "offer type", this explains it - they are "informational" so there is no expected action to complete
- Looking at channel suggests that using social media seems to increase the likelihood of people viewing offers

Looking at each factor in isolation is interesting but won't be the full picture - an offer's attractiveness will be a combination of its visibility, reward value, ease of completion and time available to complete.

And individual customers will be more or less likely to complete based on their own circumstances too.

## Modelling
A model can take into consideration all the factors when trying to predict whether a customer will respond to a particular campaign.

As the classes aren't too imbalanced, I made a basic Logistic Regression model (default parameters) as the baseline. Then I tried a basic Decision Tree (default parameters) to compare. For the best performer out of these, I built a pipeline and tuned some parameters to see if I could improve performance. Finally, I did a business evaluation to translate model performance into real life considerations of whether to use it.

# Baseline model
I didn't do anything exciting coding wise here for Logistic Regression, so the most important thing is the performance and feature ranking:
<p align="left">
  <img  src="https://github.com/jennymcphail/github.starbucks.io/blob/main/modelling1.jpg?raw=true">
</p>

The accuracy isn't too bad, performance on the test data is nearly as good as training. And the feature rank supports my earlier hypothesis - likelihood to complete is predicted by both customer profile and offer features.

The Decision Tree turned out massively over-fitted to the training data:
<p align="left">
  <img src="https://github.com/jennymcphail/github.starbucks.io/blob/main/modelling2.jpg?raw=true">
</p>

# Tuning
<p align="left">
  <img src="https://github.com/jennymcphail/github.starbucks.io/blob/main/modelling3.jpg?raw=true">
</p>

The scores have improved and the model is still performing equally well for training and test data.

# Additional Validation

As my data included all the offers in it, I wanted to check that it performs well across them all.
<p align="left">
  <img src="https://github.com/jennymcphail/github.starbucks.io/blob/main/modelling4.jpg?raw=true">
</p>
Fairly similar, although there might be an argument for building separate models per offer if Starbucks intend to stick to the same offer templates going forwards. If not, a more generic model is probably more useful.

## Real Life Model Application

Ultimately an accuracy score doesn't necessarily mean that much to the people in the marketing department. To get their buy-in for a model, I would need to think about how the model would be used in real life and prove it drives a benefit versus not using it.

This is where thinking about the confusion matrix and how that translates to outcomes is useful.

- If we contact all the people the model predicts will respond:
    * True Positives are good because the offer increases their spend
    * but False Positives will cost us money to contact and not increase their spend, and they might even get annoyed about contact for irrelevant offers (so they might even decrease their spend or take it to a competitor)
    
    
- If we don't contact the people the model predicts won't respond:
    * True Negatives are good because we save money by not marketing to them when they won't uplift their spend
    * but False Negatives are a missed opportunity, we will miss out on their incremental spend because we didn't tell them about the offer
    
To build our business case for using the model, I need the cost to market to each person and the expected incremental spend.

For example, let's imagine it costs £3.50 per person for marketing and each offer is expected to increase customer spend by £7.50.

(This is simplistic for sake of example, as in reality the channel mix for each offer would affect the costs and the offer types would likely drive different amounts of incremental spend)
<p align="left">
  <img src="https://github.com/jennymcphail/github.starbucks.io/blob/main/modelling5.jpg?raw=true">
</p>

So in this example it would make sense to try using the model.

### Taking it further

Another option would be to use the probabilities, in conjunction with campaign costs.

For example, for an expensive campaign you might only contact people with >0.7 probability of responding but for a cheap campaign, you could contact everyone with >0.3 probability of responding and just avoid those really unlikely to respond.
You could also build a model to predict the amount of incremental spend per individual and use that in conjunction with both marketing cost and likelihood to respond to decide whether to include them in each campaign.

### Final Comments
Analysis and modelling are interesting on their own, but what would make this truly valuable would be a close working relationship with the marketing department to understand their goals and constraints and use Data Science to inform their strategy.

