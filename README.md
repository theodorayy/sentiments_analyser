# Stock Symbol Information & Sentiments

> This is meant for my experimenting and educational purposes :)

In this experiment, I will attempt to analyse stock ticker symbols using sentiments garnered from the news, sentiments on Twitter, and fetch the sentiments analysed by AWS Comprehend. The app will fetch buy recommendations from Yahoo Finance as well.

This might provide a quick and basic way to display stock information at one's fingertips. [Will attempt to improve this in the near future]

## APIs used

* Twitter API
* Yahoo Finance API
* Amazon Comprehend
* Web scraping – www.finviz.com

## Get News & Tweets

**`getSymbolNews()`**

This endpoint will get the news and tweets regarding a certain stock symbol. The `fetched_news` will do a web scrape of www.finviz.com website for news and the links to the article, while tweets will return all the recent tweets within today and the previous day.

**Endpoint**

**[POST]** `/sentiments/symbol/get_news`

**Parameters**

```
{
    "symbol": "AAPL"
}
```

**Returns**

```
{
    "fetched_news": {
        "Apple Abandons Credit Card Partnership With Barclays": "https://www.fool.com/investing/2020/09/16/apple-abandons-credit-card-partnership-with-barcla/?source=eptyholnk0000202&utm_source=yahoo-host&utm_medium=feed&utm_campaign=article",
        "Apple Inc. stock underperforms Wednesday when compared to competitors": "https://www.marketwatch.com/story/apple-inc-stock-underperforms-wednesday-when-compared-to-competitors-01600287928?siteid=yhoof2",
        ...
    },
    "tweets": [
        "@elliottwaves: #Free Trial available @  https://t.co/x2kmaKA6Ui #SPY #GDX #XME #XLE #IYR #AAPL #FB #AMZN #NFLX #GOOGL #MSFT #NVDA… https://t.co/p83G3gTBRn",
        "@elliottwaves: 4 &amp; 1 hour charts for all 78 instruments are available for members viewing https://t.co/x2kmaKA6Ui #ElliottWave… https://t.co/HeOs1RKdda",
        ...
    ],
    "type": "success"
}
```

## Get Sentiments

> Use this API at your own risk. You will be billed by the second, even when you're not using it! So take care and delete the API on AWS when not using.

**`getSentiment()`**

This endpoint will get the sentiments analysed by AWS Comprehend. It will analyse the news fetched from the above function `getSymbolNews()`, and return the sentiments analysed.

**Endpoint**

**[POST]** `/sentiments/symbol/get_sentiment`

**Parameters**

```
{
    "fetched_news": {
        "Apple Abandons Credit Card Partnership With Barclays": "https://www.fool.com/investing/2020/09/16/apple-abandons-credit-card-partnership-with-barcla/?source=eptyholnk0000202&utm_source=yahoo-host&utm_medium=feed&utm_campaign=article",
        "Apple Inc. stock underperforms Wednesday when compared to competitors": "https://www.marketwatch.com/story/apple-inc-stock-underperforms-wednesday-when-compared-to-competitors-01600287928?siteid=yhoof2",
        ...
    },
    "tweets": [
        "@elliottwaves: #Free Trial available @  https://t.co/x2kmaKA6Ui #SPY #GDX #XME #XLE #IYR #AAPL #FB #AMZN #NFLX #GOOGL #MSFT #NVDA… https://t.co/p83G3gTBRn",
        "@elliottwaves: 4 &amp; 1 hour charts for all 78 instruments are available for members viewing https://t.co/x2kmaKA6Ui #ElliottWave… https://t.co/HeOs1RKdda",
        ...
    ]
}
```

**Returns**

```
{
    "overall_sentiments": "NEUTRAL",
    "sentiments": {
        "NEGATIVE": 1,
        "NEUTRAL": 9,
        "POSITIVE": 0
    }
    "type": "success"
}
```

## Get Recommendations

**`getSymbolRecommendation()`**

This will call Yahoo Finance's API to get the buy recommendations for a particular stock symbol.

**Endpoint**

**[POST]** `/sentiments/symbol/get_recommendation`

**Parameters**

```
{
    "symbol": "GOOGL"
}
```

**Returns**

```
{
    "recommendation": 0.3627906976744186,
    "recommendation_trend": {
        "buy": 25,
        "hold": 5,
        "sell": 0,
        "strongBuy": 13,
        "strongSell": 0
    },
    "type": "success"
}
```

## Installation instructions

You need to provide the API keys in `run_app.sh`, then just run `install.sh`, and `run_app.sh`!