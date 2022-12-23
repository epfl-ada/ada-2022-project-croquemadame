# CroquemADAme - YouNiverse : guiding manual to become famous on YouTube 

**Notebook: P3.ipynb inside the P3 folder**

**Website: [The Small YouTuber's Guide to Fame DataStory](https://zwierski.github.io/croquemADAme-datastory/)**



***
## Abstract
Nowadays, anyone with a smartphone and an Internet connection can become a YouTuber. Many beginners dream of success on the platform, with videos acclaimed by millions of subscribers. However, reaching fame is not as easy as it seems: indeed, with how much the number of creators has grown in the past years, it has become increasingly difficult to make an impact. The difference between famous and unknown YouTubers may lie in more than the quality of their videos. Are there any metrics in famous YouTubers' most viral videos - title, length, category, upload date - that set them apart from the rest? Could these metrics be used to create a guide for YouTubers who are starting out, and allow them to focus on the actual content with the certainty that other objective parameters are helping their success? 

## Table of Contents
1. [Research Questions](#research-questions)
2. [Methods](#methods)
3. [Organization within the team](#organization-within-the-team)


***
## Research questions
* How to grow on YouTube ?
* What are the best objective parameters of a video to improve performance ? 
* Do the title / category / length of video / tags / date of upload / frequency of upload play a role on channel growth?
* If so, what are most important parameters and their best values to improve channel growth? 
* Is there a parameter that has significantly more effect than the others ? 

***
## Methods

#### 1. Data Storage 
In <code>scoring_ent_channels_15p.ipynb</code> the following datasets are imported from [YouNiverse](https://github.com/epfl-dlab/YouNiverse):
- <code>df_channels_en.tsv.gz</code> (5,8 Mo)
- <code>df_timeseries_en.tsv.gz</code> (557,7 Mo)
- <code>_raw_yt_metadata.jsonl.zst</code> (14,3 Go)

<code>df_channel</code> and <code>df_timeseries</code> are filtered following the first point of [**2. Data processing**](https://github.com/epfl-ada/ada-2022-project-croquemadame#2-data-processing) and saved to new less voluminous files:
- <code>ent_channels.tsv.zip</code> (134 Ko)
- <code>ent_timeseries.tsv.zip</code> (12,3 Mo)

In `metadata_processing` <code>_raw_yt_metadata</code> is decoded line by line using the <code>zstd</code> library and only channels in `s_df_channels` are kept. We save the new DataFrame in 6 different parts of 1.000.000 videos each to have handleable files:
- <code>ent_metadata#.tsv.zip</code> (~76 Mo)

The 6 smaller files are grouped together to form one final file in `concat_metadata.ipynb`:

- <code>ent_metadata.tsv.zip</code> (411,9 Mo)

We are making sure each final <code>ent_*file*.tsv.zip</code> share the same channels.

* **Explanation of our different notebooks**

In the file P3 we have : 

<code>scoring_ent_channels_15p.ipynb</code>  : where we calculated the score of the channels and exported only the channels corresponding to our preprocessing mentioned before 

<code>metadata_processing.ipynb</code>  : Where we read the metada and export the coresponding videos informations for the channels selected before. We save the new DataFrame in 6 different parts of 1.000.000 videos each to have handleable files:
- <code>ent_metadata#.tsv.zip</code> (~76 Mo)

<code>concat_metadata.ipynb</code> : Where we concatenate the 6 metadata to form one file  

<code>P3.ipynb</code> : where we have all our analysis 
#### 2. Data processing 
* **Focus en Entertainment category**

For our analysis, we decided to focus on the "Entertainment" category as it is the one with the most channels. By doing so, we are making sure the channels target the same audience and share the same objectives.

* **Scoring each channel**

The goal of our analysis is to compare channels that are growing fast and channels that are not, to extract some features that could help us understand why some channels are more successful than others.

In the previous versions of our analysis, we used many different methods to select our channels. 

*Method 1* -
At first we selected only channels that started with less than 10k subscribers, and finished with more than 500k, to ensure a important growth. We then compared their videos during their buzzing period and videos before they buzzed, to see if there was a difference in the parameters of the videos. This was unsatisfying as we did not provide any explanation for selection of the thresholds.

*Method 2* -
Then we tried to attribute a score to each channel, based on the number of subscribers it had at the beginning and at the end of their timeseries period. It turned out it was unsatisfying as well, as we found out that the timeseries dataset is not complete, and that we cannot base our analysis on it. For example, almost all channels start with a non-zero number of subscribers or views, falsifying our results.

*Method 3* -
Another method we tried is to attribute a growth-score based on the proportion of new subscribers a channel gain every week. We preselected channels created after 2015. We then selected the 25% top-scored and 25% bottom-scored channels. Still unsatisfying, as channels going from 10k to 12k in two timeseries weeksn will have a higher score than channels going from 5k to 20k in 8 weeks, even if the second channel has a better growth to analyse. Also because many data is missing in the timeseries dataset, many channels created in 2016 will have their timeseries period starting in 2019, and will have a very high score.

*Final method* -
Finally we decided not to use the timeseries dataset for the scoring of the channels. We are scoring each channel based on the number of subscribers it has in 2019, and its date of creation. We are using the following formula to do so:

$$\text{score} = \dfrac{\text{subs in 2019}}{\text{days since creation}}$$

A channel with 100k subscribers in 2019 and created in 2015 will have a score of 100k/1461 = 55, while a channel with 100k subscribers in 2019 and created in 2018 will have a score of 100k/365 = 273.

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
  * We will also compute the importance of the features related to the title only

* **Analysis of the tags**
  * Compute the most used tags for each channel
  * Compare the most used tags over all buzzing channels and to the most used tags over all quiet channels
  * Use a wordcloud to see the most used tags for each group

* **Lexical analysis on the title** 
  * Analyse the topics for the title of buzzing channels and quiet ones 
  * Venn diagram of the 15 most used topics to see with ones are specific to the buzzing and quiet channels 

* **Analysis of the better period to post a video**
  * Plot of the number of buzzing videos in function of the month of the year. 
  * It will give an idea of interest of spectators in function of the period.

#### 4. Visualization 

* Example of a youtuber: plot evolution of subscribers and its videos features

* Plot of the features importances 

* Boxplot of the parameters for buzzing and quiet channels 

* Plot of the most represented topics in the title 

* Venn diagram of the most used topics for buzzing and quiet channels 

* Wordcloud of the most used tags 

* Histogram of the upload_date of the buzzing video in function of the months
 


#### 5. GitHub site building and data story redaction.

##### **The Small YouTuber's Guide to Fame DataStory**

We will follow the journey of a small youtuber asking for advices and buzzing on YouTube : [Link to our website](https://zwierski.github.io/croquemADAme-datastory/)

***
## Organization within the team
A list of internal milestones up until project Milestone P3.
* Ludovic : Preprocessing, Data analysis, data story & web site building
* Margaux Z : Data story and web site building 
* Carolina : Data story
* Margaux R : Data analysis, visualization, data story & web site building


