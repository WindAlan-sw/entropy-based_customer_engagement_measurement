## An entropy-based measurement approach on social media customer engagement using natural language processing (Under building)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)<br>

In recent years, the importance of social media engagement has been growing with a valuable communication channel with customers for luxury brands. Existing studies have applied big data analytics and text mining techniques to gain strategical insights from social media platforms. However, challenges such as uncertain data source and complex levels of customer engagement hinder us from extending existing methods to a broader application scenario. This paper focuses on these challenges, introducing the concept of Shannon entropy to the domain of social media engagement in an objective manner. To validate the effectiveness of this method, 18, 964 samples were collected from 6 luxury brands, along with their public metrics. The results show that interactive contributions including comments and quotes, provide a more influential weights in the process of determining the customer engagement performance of a single piece of social media content. In addition, the validation test proves a positive correlation between examined brand engagement performance and their brand value. Also, the concept of attributions of social media marketing activities proposed by Kim and Ko (2012) have been incorporated to study the impact of brand social media strategy influences customer engagement. The result of this research is consistent with the previous study conducted by Liu et al. (2021). The findings indicate an improved customer-engagement measurement approach is significant to evaluate the social media performance of brands, ultimately helping brands to improve their social media strategy.

## Framewrok
#### Scraping data
Coding part see [tweetscrape.py](https://github.com/WindAlan-sw/Measuring_customer-engagement_through_entropy/blob/main/tweetscrape.py)\
we used Tweepy, a Python library that enables accessing data through the Twitter API (Roesslein, 2009)[^2]. This library has been systematically applied to the study of social media operations, for example, Hasson, Piorkowski, and McCulloh (2019)[^3]. In this task, we prepare **7 columns**:
  1. retweet_count
  2. reply_count
  3. like_count
  4. quote_count
  5. Tweet
  6. time
  7. brand
  
Front four dimensions of 'retweet_count', 'reply_count', 'like_count', 'quote_count' can be obtained through public metrics setting in tweepy. The enveloped form scraping tweet function can be used through:

```
import tweetscrape

name='Burberry'
start_time = '2017-01-01T00:00:00Z'
end_time = '2022-07-31T00:00:00Z'
max_num = 10
output_name = 'Burberry'
tweetscrape.main(name,start_time,end_time,max_num,output_name)

```
Usage details have been written in tweetscrape.py. Note that the area below should be obtained through own creation:
```
key = ''
secret = ''
token = ''
token_secret = ''
b_token = ''
```

A successful running will have a result similar to below:
```
--------------TwitterID of target account----------------
The ID of user Burberry is: 47459700
----------------------Tweet_info-----------------------
The tweet_id of the first scraped tweet: 1553379987784249344
The public metrics of the first tweet: {'retweet_count': 80, 'reply_count': 33, 'like_count': 224, 'quote_count': 2}
The content of the first tweet: Punctuated with the #TBMonogram, inspired by our founder Thomas Burberry, our TB Bucket Bag marries structured sartorialism with pared-back ease

Shop the bag at https://t.co/e82iUQLoyu

#Burberry https://t.co/OcjkLmwaKB
The time of the first tweet: 2022-07-30T14:00:24.000Z
------------------ Dictionary Info ----------------------
Here is the first line of the New_dict... ...
{'retweet_count': 80, 'reply_count': 33, 'like_count': 224, 'quote_count': 2, 'Tweet': 'Punctuated with the #TBMonogram, inspired by our founder Thomas Burberry, our TB Bucket Bag marries structured sartorialism with pared-back ease\n\nShop the bag at https://t.co/e82iUQLoyu\n\n#Burberry https://t.co/OcjkLmwaKB', 'Time': '2022-07-30T14:00:24.000Z'}
... ...
Programming is finished.
```
#### TASK 2 Calculating customer-engagement score using Shannon entropy
The code part see [entropy.py](https://github.com/WindAlan-sw/Measuring_customer-engagement_through_entropy/blob/main/entropy.py)\
The dataset for this project is stored in [data](https://github.com/WindAlan-sw/Measuring_customer-engagement_through_entropy/tree/main/data) - [All_brands.csv](https://github.com/WindAlan-sw/Measuring_customer-engagement_through_entropy/blob/main/data/All_brands.csv).\
Before calculating the customer-engagement score, if doing the scatter plotting, we can find long-tail because practically, social media content could have extremely high range between the normal and best-performing. See,

<img width="479" alt="Screenshot 2022-11-12 at 22 04 53" src="https://user-images.githubusercontent.com/76271974/201496350-67934986-6cb5-4473-80ba-25bd02753c73.png">


Therefore, here we propose to adopt the root transformation to before normalization, as you can find the root transformation method by Cousineau and Chartier (2010)[^4]. The enveloped root transformation result is:

<img width="470" alt="Screenshot 2022-11-12 at 21 34 57" src="https://user-images.githubusercontent.com/76271974/201495468-6cf03260-1eff-4b24-a0f3-d51a54155d01.png">

A more rational and observable value range for further analysis. Then we are going to apply entropy algorithm to get the objective weight scheme for four public metrics. Fortunately, we have enveloped the entropy calculation algorithm. You can run the algorithm by the code below:
```
# score_calculation(df, m,n, y_min, y_max, lowest)
sha_score = entropy.score_calculation(df,0,4,0.01,1,0)
```
The first parameter is dataframe waiting for input, and next two parameter m,n indicate the index location in that dataframe. y_min and y_max determine the value range of a normalized dataset, and lowest gives a toralance rate to the data range. Let's have a look at the shannon entropy method applied to All_brands.csv:

<img width="619" alt="Screenshot 2022-11-12 at 22 30 33" src="https://user-images.githubusercontent.com/76271974/201497060-a5476b6f-b0b7-41c4-90ee-56127fb7ac82.png">

__Note that in Shannon-entropy calculation, the lowest boundary of a column should be greater than 0 since the log function should be applied to each row and dominator could not be 0__

Here we propose another entropy calculation method that considers less mainstream ideas to promote fairness. Details refer to [^5]. This function can be realize through the following code:

```
CRITIC_score = entropy.CRITIC(df_2,0,4,0.01,1,0)
```

Let's see the weights determined by CRITIC entropy method:


<img width="620" alt="Screenshot 2022-11-12 at 23 07 58" src="https://user-images.githubusercontent.com/76271974/201498042-22c52836-6c09-4b58-ab73-57c89522f83c.png">


     
#### Evaluating Tweets by EITC framewrok
This step is aiming at evaluating each Tweet's attributions for further finding managerial insights through EITC framewrok. The attribution will require NLP (Natural Language Processing) package to obtain the following attributions of a tweet record: 

    1. Entertainment
    2. Interaction
    3. Trend
    4. Customization

Entertainment and Trend can be quantified through matching a dictionary while interaction and customization, in this project, are defined through the frequency of using hashtag and mention in each tweet. Functions have been built in getting the number of hashtags and mentions in this project. Therefore, we can directly get two dimensions of Interaction and Customization:
```
tweet_df['Interaction']= (1+tweet_df['hashtag_count']+tweet_df['mention_count'])
tweet_df['Customization'] = tweet_df['mention_count']
```
To fix the rest two dimensions of Entertainment and Trendiness, we create the two dictionaries abstracted from Liu, Shin and Burns (2019)[^1].
```
Entertainment=["amuse","amuses", "amused", "amusing", "amusement", 
               "anticipated", "anticipation","anticipating", "captivate", 
               "captivating", "captivated", "captivation", "captivates", 
               "clever","enjoying", "enjoyment", "enjoyable", "compelling",
               "entertain", "entertainment","entertaining", "feast", 
               "festival", "festive", "film", "fun", "funny", "funnier", 
               "funniest", "good time","hilarious", "humor", "humorous", 
               "hysterical", "hysterically", "interesting", "interested",
               "laugh", "laughter", "laughing", "mesmerizing", "mesmerized", 
               "pageantry", "performance","performer", "pleasure", "recreation", 
               "red carpet", "relaxing", "relaxation", "ridiculously",
               "screaming", "theatre", "thrill", "thrilling", "thrilled", 
               "witty"]
Trendiness=["best", "catwalk", "chic", "classy", "contemporary", "cool", 
            "creative", "dressy", "elegant", "elegantly","fabulous", "fabulously",
            "famous", "fashion", "fashionable", "first class", "glamor","glamourous", 
            "gorgeous", "gorgeously", "icon", "iconic", "in vogue", "influential",
            "innovative","innovator", "innovating", "inspiring", "inspiration", 
            "latest", "leading", "leader", "luxurious","luxury", "modish", "modern", 
            "newest", "on-trend", "pioneering", "popular", "renowned",
            "styles", "stylish", "supermodel", "superstar", "top", "trend", 
            "trends", "trendy", "trend-setter"]
```
We can build functions that return '1' for a tweet if it  cotains a word from Entertainment dictionary, so does for a situation of Trendiness dictionary. After getting four dimensions, we use them to validate the customer-engagement score calculated from entropy.

#### TASK4 Statistical Validating
This part explores the statistical replationship between calculated customer-engagement perfromance score from public metrics of tweets , and EITC attributions from tweet content. To ensure a fixed-effect model can be applied to these variables, the Hasuman test applied to it. Since the interaction is at a significant level (< 5%), we progress the below formulation:
```
score(dep. var.) ~ Entertainment + Interaction + Trendiness + Customization 
```
Compared the OLS model result done by Shannon-entropy and CRITIC-entropy respectively, we find the CRITIC-score shows higher signicance level, meaning that the CRITIC method indicates a stronger statistical relationship and better simulation. 

*Note that the dataset has high variance and the model result we discuss here should have been the optimal model. Another reason accounting for the 0.05 R-squared level is that our result is not built on aggregation data, so showing high discrete attribution.*

A conclusion here is that an entropy algorithm that considers fairness is more likely to reflect the relationship with real performance of social media marketing. Below paste the OLS Regression result of model done by CRITIC customer-engagement score:

<img width="651" alt="Screenshot 2022-11-12 at 23 42 23" src="https://user-images.githubusercontent.com/76271974/201498910-cdb286f7-c5b2-4948-9d09-dd1b895958e4.png">

Next, we will introduce improve this result by implementing dictionaries of Entertainment and Trendiness. Therefore, here uses wordcloud to capture the high-frequency words used in tweet samples but not appeared in original dictionaries. And here is the result after adding those keywords reflecting trendiness and entertainment:

```
Entertainment_2=["amuse","amuses", "amused", "amusing", "amusement", 
               "anticipated", "anticipation","anticipating", "captivate", 
               "captivating", "captivated", "captivation", "captivates", 
               "clever","enjoying", "enjoyment", "enjoyable", "compelling",
               "entertain", "entertainment","entertaining", "feast", 
               "festival", "festive", "film", "fun", "funny", "funnier", 
               "funniest", "good time","hilarious", "humor", "humorous", 
               "hysterical", "hysterically", "interesting", "interested",
               "laugh", "laughter", "laughing", "mesmerizing", "mesmerized", 
               "pageantry", "performance","performer", "pleasure", "recreation", 
               "red carpet", "relaxing", "relaxation", "ridiculously",
               "screaming", "theatre", "thrill", "thrilling", "thrilled", 
               "witty",
                "special", "fashion show", "celebrate", "signature"]
Trendiness_2=["best", "catwalk", "chic", "classy", "contemporary", "cool", 
            "creative", "dressy", "elegant", "elegantly","fabulous", "fabulously",
            "famous", "fashion", "fashionable", "first class", "glamor","glamourous", 
            "gorgeous", "gorgeously", "icon", "iconic", "in vogue", "influential",
            "innovative","innovator", "innovating", "inspiring", "inspiration", 
            "latest", "leading", "leader", "luxurious","luxury", "modish", "modern", 
            "newest", "on-trend", "pioneering", "popular", "renowned",
            "styles", "stylish", "supermodel", "superstar", "top", "trend", 
            "trends", "trendy", "trend-setter",
              "iconic", "classic", "bold", "worn", 
              "haute", "styled", "special", "model", 
              "colorful", "tailoring", "bontique","inspired", "spirit", "creation"]
```
From the result in [project.ipynb](https://github.com/WindAlan-sw/Measuring_customer-engagement_through_entropy/blob/main/project.ipynb), the simulation and signicance of relationship have been increased. 

To compare the effectiveness of social media marketing of different objectives, here we plot the score distributions of six brands.

<img width="533" alt="Screenshot 2022-11-12 at 23 49 25" src="https://user-images.githubusercontent.com/76271974/201499134-ee222536-5c7e-4319-834d-4cd9697bd6ca.png">

By proposed entropy-based customer engagement score, a clear ranking could be obtained among six brands, to better distinguish them, a ratio histogram plotted:

<img width="549" alt="Screenshot 2022-11-12 at 23 49 31" src="https://user-images.githubusercontent.com/76271974/201499185-a00a2b07-02e3-4fa6-bf98-1883c52c508b.png">
**Note that Group1 in range [0,28],
Group 2 in range (28,31.28],
Group 3 in range (31.28,34.18],
Group 4 in range (34.18, 100] based on calibration.**

Based on results above, customer engagement score calculated form Shannon-entropy shows high discriminatory power in differentiating target luxury brands. Generally, the weak performance group includes Armani and Burberry with over 40% of their engagement score belongs to group 1. Chanel and LV can be classified into the same group which shows better social media marketing performance, over 80% of their social media content were evaluated a higher score belonging to group 3&4. 




[^1]:Liu, X., Shin, H. and Burns, A.C. (2019). Examining the Impact of Luxury brand’s Social Media Marketing on Customer engagement: Using Big Data Analytics and Natural Language Processing. Journal of Business Research, 125, pp.815–826. doi:10.1016/j.jbusres.2019.04.042.
[^2]:Roesslein, J.(2009), Tweepy. An Easy-To-Use Python Library for Accessing Twitter API. http://www.tweepy.org/
[^3]:Hasson, S.G., Piorkowski, J. and McCulloh, I. (2019). Social media as a main source of customer feedback. Proceedings of the 2019 IEEE/ACM International Conference on Advances in Social Networks Analysis and Mining. doi:10.1145/3341161.3345642.
[^4]:Cousineau D., Chartier, S. (2010). Outliers detection and treatment: a review. International Journal of Psychological Research, 3 (1), 58-67.
[^5]:Diakoulaki, D., Mavrotas, G. and Papayannakis, L. (1995). Determining objective weights in multiple criteria problems: The critic method. Computers & Operations Research, [online] 22(7), pp.763–770. doi:10.1016/0305-0548(94)00059-H.
