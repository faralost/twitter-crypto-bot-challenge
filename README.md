# Twitter Bot Challenge

## To start

**Send your resume, Github username and a link to this challenge to [challenge-submission@blockshop.org](mailto:challenge-submission@blockshop.org). Then we'll add you to collaborators and share all necessary credentials.**

## What's next?

If you are ready with your branch - create a pull request and assign it to @sofiasedlova and @darknessest for review. As soon as we get a good enough solution from a candidate, we start the interviewing process.


### Github Secrets for Actions

#### Description

For this challenge you have a Authentication Tokens (Access Token and Secret) and Consumer Keys (API Key and Secret), 
so you can only use [OAuth 1.0a](https://developer.twitter.com/en/docs/authentication/oauth-1-0a) for authentication.

Authentication Tokens are stored in `TW_ACCESS_TOKEN` and `TW_ACCESS_TOKEN_SECRET` respectivelly.

Consumer Keys are stored in `TW_CONSUMER_KEY` and `TW_CONSUMER_KEY_SECRET` respectively.

The MongoDB credentials are stored in `MONGODB_USER` and `MONGODB_PASSWORD`, also the cluster's address is stored in `MONGO_DB_ADDRESS`.

Make sure you are using the latest [PyMongo](https://github.com/mongodb/mongo-python-driver) with srv support, you can install it with:

```bash
pip install -U 'pymongo[srv]'
```

You can find a Github Action template [here](.github/workflows/gh-action-template.yml), please make sure you copy it to your branch and change the name of the branch in the yaml file. This will help the action's execution. 


#### Usage

To pass Github Secrets to your action, you need to specify the secrets and their corresponding names like following:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:

      - name: Run Python Code
        env:
          MONGODB_USER: ${{ secrets.MONGODB_USER }}
          TW_BEARER_TOKEN: ${{ secrets.TW_BEARER_TOKEN }}
        run: |
            python3 main.py
```



You can access environment variables in Python like that:

```python
import os

bearer_token = os.environ["TW_BEARER_TOKEN"]
```


URI for connecting to the MongoDB cluster can be constructed in the following way:

```python
from pymongo import MongoClient

user = os.environ["MONGODB_USER"]
password = os.environ["MONGODB_PASSWORD"]
address = os.environ["MONGO_DB_ADDRESS"]

uri = f"mongodb+srv://{user}:{password}@{address}"
client = MongoClient(uri)

```

The goal of this challenge is to build a Python Twitter bot using GitHub Actions and MongoDB.

Required functionality
A Python function will compose and post a tweet with the latest values from MongoDB time series collections using the logic described below.

Algo
query ohlcv_db for the top 100 pairs by compound volume
query posts_db for the latest documents corresponding to those 100 pairs
sort results by the oldest timestamp to find the pairs that haven't been posted for a while, then corresponding volume to find the biggest markets among them and select the pair_to_post
compose message_to_post for the pair_to_post with corresponding latest volumes by market values from ohlcv_db using this example:
```
Top Market Venues for BTC-USDC:
Binance 30.10%
Coinbase 20.20%
Kraken 10.30%
Bitstamp 5.40%
Huobi 2.50%
Others 31.5%
```
keep similar tweets in one thread. if pair_to_post tweets already exists in posts_db, post tweet to the corresponding Twitter thread. else, post a new tweet.
add your message_to_post to posts_db
Important notes
Send your resume and a link to this challenge to challenge-submission@blockshop.org. Then we'll add you to collaborators and share all necessary credentials.
Create your own branch in this repository and use iterative approach. We want to see your progress as much as we want to see the result.
Think about tests and error handling. We will be stress testing your code by corrupting databases and revoking access permissions.
Limit Twitter API usage, rely on MongoDB to save the state.
Leverage MongoDB time series optimizations to save computational resources.
As soon as we get a good enough solution from a candidate, we start the interviewing process. If you are ready with your branch - create a pull request and assign it to @sofiasedlova and @darknessest for review.
