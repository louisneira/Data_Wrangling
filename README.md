# Data_Wrangling Sample Project

The purpose of this document is to outline the steps taken to obtain and clean @WeRateDogs tweet data. 

# Obtaining Data

Three different data sets were obtained for this assignment. First, an internally stored file, twitter_archive_enhanced.csv, was read into a Pandas DataFrame. Next, the image data file image_predictions.tsv was downloaded from a website using the Python response library. Last, additional tweet data (retweet counts and favorite counts) were obtained via the Twitter API for Python, Tweepy.

# Assessing and Removing Invalid Data

With the data obtained, the data were assessed first for valid data. The project specified that retweets should be dropped, so these were dropped. Then replies were assessed to ascertain their validity. Replies did not appear to be original ratings, so these data were also dropped. 

Retweet and reply data were recognizable because several columns in the data were dedicated to these types of tweets. Two columns contained non-null data only if the tweet was a reply, and three other columns only contained non-null data if the tweet was a retweet. After removing tweets and retweets, the five columns related to replies and retweets contained zero non-null entries and therefore provided no information for the remaining tweets. Therefore, these columns were dropped.

Image data were assessed for validity. The data contained a set of three predictions for the most likely dog species, where one column, p1, contained the species name of the most likely dog species based on the prediction method used. Associated with p1 were columns p1_conf, which was a probability that the guessed species is correct, and p1_dog, a Boolean column that is true if p1’s guess is a dog species. Because the focus of this analysis is dogs, any entries where p1_dog is false were removed. This removed a total of 543 rows from the images data frame.

# Data Reorganization

The data frame containing the tweet data and the count data obtained from the API call were then merged since both tables contain data about the tweets. This was done using an inner join, so not only were the related data sets merged into one data frame, but any tweets where tweet data was not available via the API call were dropped, so that all tweets in the data frame contained valid count data. 

Tweet IDs were used to remove any tweets that did not have corresponding image data as well as images that did not have a corresponding tweet, so that every tweet had a corresponding image and every image a corresponding tweet.

# Additional Steps to Tidy and Clean Data

Dog stage data were actually stored in four separate columns, one for each stage. These data were combined into one column, ‘dog_stage’ that contained one of five possible values: ‘None’, ‘doggo’, ‘floofer’, ‘puppo’, or ‘pupper.’ While performing this task, it was noted that 9 columns had multiple dog_stage values. Upon further inspection, each one had an extra rating of ‘doggo’ so the ‘doggo’ rating was removed so that each entry in the ‘dog_stage’ column had one of the five possible values.

Column data types were not all appropriate based on the data contained within. The ‘tweet_id’ column in the data frame containing tweet information was of type int. However, because it is only an identifier and there is no need to perform mathematical computations with tweet IDs, these are more appropriate as strings. The timestamp column in the tweet information data frame was of type string when datetime may be more appropriate. Both of these column data types were modified. 

Some ratings, particularly where there were multiple fractions in a tweet’s text, had not been accurately recorded. The fractions were extracted from a text and then compared to actual ratings. Based on this analysis, a few incorrect ratings were updated. Some names were also not recorded correctly. Visual inspection of names ‘None’, ‘an’ and ‘just’ led to modifications of a small number of names.

