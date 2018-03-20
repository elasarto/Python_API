
Observable Trends:

- Based on the recent tweet data, there appear to be many more negative sentiments from Fox News than other media outlets in the dataset. Sentiments for this media outlet range much closer to the -1 maximum value than others.
- CNN, CBS, and The New York Times have more instances of positive sentiments than the rest of the media outlets in the dataset; higher instances of compound values over 0.75.
- CNN and CBS News have a seemingly even distribution of types of sentiments in tweets, neither news outlet heavily skews positive or negative. This data shows that CNN has a slightly more positive compound sentiment value than CBS News. 




```python
# Dependencies
import tweepy
import json
import numpy as np
import pandas as pd
import seaborn as sns
import time
import matplotlib.pyplot as plt
from config import consumer_key, consumer_secret, access_token, access_token_secret
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()

# Setup Tweepy API Authentication
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
# get tweets
# twitter accounts
target_users = ["@CNN", "@nytimes", "@FoxNews", "@BBCNews", "@CBSNews"]

sentiment_data = []

for target_user in target_users:

    counter = 1
    
    for x in range(5):
    
        public_tweets = api.user_timeline(target_user)

        for tweet in public_tweets:
        
            #print(json.dumps(tweet, sort_keys=True, indent=4, separators=(',', ': ')))
            #break
    
            # Run Vader Analysis on each tweet
            compound = analyzer.polarity_scores(tweet['text'])["compound"]
            pos = analyzer.polarity_scores(tweet['text'])["pos"]
            neu = analyzer.polarity_scores(tweet['text'])["neu"]
            neg = analyzer.polarity_scores(tweet['text'])["neg"]
            tweets_ago = counter
        
                # Add sentiments for each tweet into an array
            sentiment_data.append({"Account Name": tweet['user']['name'],
                                "Date": tweet['created_at'],
                                "Tweet Text": tweet['text'],             
                                "Compound": compound,
                                "Positive": pos,
                                "Negative": neu,
                                "Neutral": neg,
                                "Tweets Ago": counter})
        
                # Add to counter 
            counter = counter + 1
```


```python
sentiments_df = pd.DataFrame.from_dict(sentiment_data)
sentiments_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account Name</th>
      <th>Compound</th>
      <th>Date</th>
      <th>Negative</th>
      <th>Neutral</th>
      <th>Positive</th>
      <th>Tweet Text</th>
      <th>Tweets Ago</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CNN</td>
      <td>0.7772</td>
      <td>Mon Mar 19 23:21:23 +0000 2018</td>
      <td>0.715</td>
      <td>0.000</td>
      <td>0.285</td>
      <td>RT @TheLeadCNN: We're celebrating 5 years of #...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CNN</td>
      <td>0.2500</td>
      <td>Mon Mar 19 23:20:00 +0000 2018</td>
      <td>0.905</td>
      <td>0.000</td>
      <td>0.095</td>
      <td>Trump lawyer Michael Cohen jokes about Stormy ...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CNN</td>
      <td>0.0000</td>
      <td>Mon Mar 19 23:10:06 +0000 2018</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>With just days before a potential shutdown, ne...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CNN</td>
      <td>-0.4019</td>
      <td>Mon Mar 19 23:07:04 +0000 2018</td>
      <td>0.816</td>
      <td>0.184</td>
      <td>0.000</td>
      <td>An anti-abortion, conservative Democrat fights...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CNN</td>
      <td>0.0000</td>
      <td>Mon Mar 19 23:01:07 +0000 2018</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>What you need to know about Facebook's data de...</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
sentiments_df = sentiments_df[["Account Name", "Date", "Tweet Text", "Compound", "Positive",
                               "Negative", "Neutral", "Tweets Ago"]]

sentiments_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account Name</th>
      <th>Date</th>
      <th>Tweet Text</th>
      <th>Compound</th>
      <th>Positive</th>
      <th>Negative</th>
      <th>Neutral</th>
      <th>Tweets Ago</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CNN</td>
      <td>Mon Mar 19 23:21:23 +0000 2018</td>
      <td>RT @TheLeadCNN: We're celebrating 5 years of #...</td>
      <td>0.7772</td>
      <td>0.285</td>
      <td>0.715</td>
      <td>0.000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CNN</td>
      <td>Mon Mar 19 23:20:00 +0000 2018</td>
      <td>Trump lawyer Michael Cohen jokes about Stormy ...</td>
      <td>0.2500</td>
      <td>0.095</td>
      <td>0.905</td>
      <td>0.000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CNN</td>
      <td>Mon Mar 19 23:10:06 +0000 2018</td>
      <td>With just days before a potential shutdown, ne...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CNN</td>
      <td>Mon Mar 19 23:07:04 +0000 2018</td>
      <td>An anti-abortion, conservative Democrat fights...</td>
      <td>-0.4019</td>
      <td>0.000</td>
      <td>0.816</td>
      <td>0.184</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CNN</td>
      <td>Mon Mar 19 23:01:07 +0000 2018</td>
      <td>What you need to know about Facebook's data de...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
sentiments_df.to_csv("media_sentiment_data.csv", encoding="utf-8", index=False)
```


```python
sns.set()
sns.lmplot(x='Tweets Ago', y='Compound', data=sentiments_df, hue='Account Name', fit_reg=False)

plt.title("Sentiment Analysis of Tweets for Media Outlets (%s) %s" % (time.strftime("%x"), ""))

plt.ylabel("Tweet Polarity")
plt.xlabel("Tweets Ago")

plt.grid(True)
plt.xlim([100, 0])
plt.ylim([-1, 1])

plt.savefig("Sentiment_Analysis_Tweets.png", dpi=150)

plt.show()
```


![png](output_6_0.png)



```python
sns.set()
ax = sns.barplot(x='Account Name', y='Compound', data=sentiments_df, ci=None)

plt.title("Overall Tweet Sentiments for Media Outlets (%s) %s" % (time.strftime("%x"), ""))
plt.ylabel("Tweet Polarity")
plt.xlabel("Tweets Ago")

plt.ylim(-0.18, 0.05)

#get labels for bars
for p in ax.patches:
    height = p.get_height()
    ax.text(p.get_x()+p.get_width()/2.,
            1.05*height,
                '{:1.2f}'.format(height),
            ha="center") 

plt.savefig("Media_Sentiment_Overall.png", dpi=150)
plt.show()
```


![png](output_7_0.png)

