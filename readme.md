# CroquemADAme - YouNiverse : guiding manual to become famous on YouTube 
***
## Abstract
Nowadays, becoming a YouTuber is accessible to anyone with a smartphone and an Internet connection. Growing into a successful person on this platform is therefore the dream of many. However, this is not as easy as it seems. With the emergence of many users, it has become increasingly difficult to make your own place. Therefore, we would like to create a guideline for small YouTubers (our analysis is focus on youtubers that started with 10k to 15k subscribers) - how to choose the perfect video duration, what tags to use, the best contents, the best period to post, the perfect title to attract new viewers, etc. - to allow them to focus on the actual content with the certainty that other objective parameters are potentiating the success. 

## Table of Contents
1. [Research Questions](#research-questions)
2. [Methods](#methods)
3. [Proposed Timeline](#proposed-timeline)
4. [Organization within the team](#organization-within-the-team)


## Research questions
***
* How to grow fast on YouTube ?
* What are the best objetive parameters of a video to improve performance ? 
* Do the title / category / length of video / tags / date of upload / frequency of upload play a role on channel growth?
* If so, what are most important parameters and their best values to improve channel growth? 
* Is there a parameter that has significantly more effect than the others ? 

## Methods
***

#### 1. Data Storage 
In <code>P2_preprocessing.ipynb</code> the following datasets are imported from [YouNiverse](https://github.com/epfl-dlab/YouNiverse):
- <code>df_channels_en.tsv.gz</code> (5,8 Mo)
- <code>df_timeseries_en.tsv.gz</code> (557,7 Mo)
- <code>_raw_yt_metadata.jsonl.zst</code> (14,3 Go)

<code>df_channel</code> and <code>df_timeseries</code> are filtered following the first point of [**2. Data processing**](https://github.com/epfl-ada/ada-2022-project-croquemadame#2-data-processing) and saved to new less voluminous files:
- <code>ent_channels.tsv.zip</code> (134 Ko)
- <code>ent_timeseries.tsv.zip</code> (12,3 Mo)

<code>_raw_yt_metadata</code> is decoded line by line using the <code>zstd</code> library and only channels in `s_df_channels` are kept. We save the new DataFrame in 6 different parts of 1.000.000 videos each to have handleable files:
- <code>ent_metadata#.tsv.zip</code> (~76 Mo)

The 6 smaller files are grouped together to form one final file:

- <code>ent_metadata.tsv.zip</code> (411,9 Mo)

We are making sure each final <code>ent_*file*.tsv.zip</code> share the same channels.


#### 2. Data processing 
* **Focus en Entertainment category**

For our analysis, we decided to focus on the "Entertainment" category as it is the one with the most channels. By doing so, we are making sure the channels target the same audience and share the same objectives.

* **Scoring each channel**

We are scoring each channel based on the number of subscribers it has and its date of creation. We are using the following formula to do so:

$$\text{score} = \dfrac{\text{subs in 2019}}{\text{days since creation}}$$

* **Selecting channels according to their score**

To compare buzzing channels and quiet channels, we are selecting the 15% channels with the highest score and the 15% channels with the lowest score. We are left with 2 groups of 3.300 channels and  around 5,7M videos which is enough for our analysis.

#### 3. Analysis of the data 


* **Comparison of the parameters of buzzing channels and quiet channels** 

Now we have channels that evolve a lot and other that did not. We will name them as buzzing and quiet channels. 
For each videos of each channel, we will compute the parameter analysis : 
- calculate the number of words in the title
- calculate number of tags 
- calculate if there is tags
- calculate if they used the lexical field of featuring in the title
- calculate if they used capital letters in the title
- calculate if they used pronouns(first person of singular, first person of plural, second person, third person of singular, third person of plural) in the title
- calculate if they used positive or negative words in the title
- The duration of the video
- Calculate the week frequency of uploaded videos 

Then we computed the mean of each channel of those parameters.

We then obtained a dataset with the following columns :
`mean_capital_title`,`mean_numwords`,`mean_feats`,`mean_duration`,`mean_numtags`,`mean_is_tags`,`week_freq`,`mean_first_person_singular`,`mean_first_person_plural`,`mean_second_person`,`mean_third_person_singular`,`mean_third_person_plural`,`mean_positive`,`mean_negative`


| channel   | mean_duration | mean_numwords| mean_capital_title | mean_feats | mean_numtags | mean_is_tags | week_freq | mean_positive | mean_negative | pronouns | has_buzzed |
|-----------|---------------|--------------|--------------------|------------|--------------|--------------|-----------|---------------|---------------|---------|----------|
| channel_a |    500        |     6        |       0.1          |    0.01    |     5        |     0.5      |    0.5    |       0.4     |     0.8       |    0.2    |    0    |
| channel_b |    600        |     10       |       0.3          |    0.005   |     7        |     0.8      |     1.5   |       0.9     |     0.3       |   0.7    |    1    |
| channel_c |    650        |     13       |       0.2          |    0.02    |     9        |     0.7      |      3    |       0.7     |     0.2       |    0.5    |    1    |


* **Importance of features**

  * We will compute a random tree to see which feature is the most importance 
  * We will then compute t-test on each parameters to see wich one is significant for our analysis
  * We will also compute linear regression on the most important features 
  * We will also compute the importance of the features of the title 

* **Analysis of the tags**
  * Compute the most used tags for the buzzing and non buzing channels 

* **Lexical analysis on the title** 
  * Analyse the topics for the title of buzzing channels and quiet ones 
  * Venn diagram of the 15 most used topics to see with ones are specific to the buzzing and quiet channels 
  * Wordcloud of the most used words on the title 

* **Analysis of the better period to post a video**
  * Plot of the number of buzzing videos in function of the month of the year. 
  * It will give an idea of interest of spectators in function of the period.

#### 4. Visualization 

* Plot of the categories popularity

![image](images/nb_ch_per_cat.png)

* Plot of a youtuber (an example) : evolution of delta_subs, evolution of subscribers 

* Plot of the features importances 

* Boxplot of the parameters for buzzing and quiet channels 

* Plot of the topics in the title 

* Venn diagram of the most used topics for buzzing and quiet channels 

* Plot of the most used tags 

* Wordcloud of the most used words in title 

* Histogram of the upload_date of the buzzing video in function of the months
 


#### 5. GitHub site building and data story redaction.



## Proposed Timeline
***
18.11.22 P2 deadline

- Small data analysis

02.12.22 Homework 2 deadline

06.12.22 Do the final analysis

10.12.22 Begin of the redaction of the data story

14.12.22 Finish the code and visualizations 

19.12.22 Finish datastory

23.12.22 Milestone 3 deadline

## Organization within the team
***
A list of internal milestones up until project Milestone P3.
* Ludovic : Data analysis (Comparison, Anova), visualization
* Margaux Z : Data story and web site building 
* Carolina : Data story and web site building
* Margaux R : Data analysis (Title, categories), visualization


