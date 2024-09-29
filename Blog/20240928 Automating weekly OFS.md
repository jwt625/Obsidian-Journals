2024-09-28T21:04:17-07:00
Following ChatGPT's advice:
### 1. **Set up Twitter API Access**

- **Create a Twitter Developer Account**: You'll need to apply for access to Twitter's API. Go to [Twitter Developer](https://developer.twitter.com/en/apps) and create an app.
- **Generate API Keys**: Once the app is created, you will get API keys and bearer tokens, which you’ll use to authenticate your requests.

Signed up the free tier with the following description:

>I intend to use Twitter's API to back up my personal tweets and bookmarks for archival purposes. The goal is to create a personal record of my content, including text, images, and links, to ensure I retain access to important information I've shared or saved. Given that content on the internet can sometimes be lost, altered, or deleted over time, this backup serves as a precaution to maintain my own digital history for future reference.

- https://developer.twitter.com/en/portal/products

API documentation:
- https://developer.x.com/en/docs/x-api/tweets/manage-tweets/introduction

Manage bookmarks:
- https://developer.x.com/en/docs/x-api/tweets/bookmarks/introduction

The free tier:
![[Pasted image 20240928211947.png]]

### 2. **Install Required Libraries**

- Install the Python libraries that will help you interact with the Twitter API

Working in `GitHub\PlayGround\20240928_Twitter_API`.

```
pip install tweepy requests

```
- [x] installed `tweepy requests`

### 3. **Authenticate and Access Bookmarks**

- Twitter’s v2 API does not currently have direct support for accessing bookmarks. You’ll likely need to use a **user-scripted solution** or leverage third-party automation tools.
- **Workaround**: If bookmarks aren’t accessible, you might need to favorite the tweets instead, as you can access liked tweets using the API:

```python
import tweepy

client = tweepy.Client(bearer_token="YOUR_BEARER_TOKEN")

# Get user’s liked tweets (simulating bookmarks)
liked_tweets = client.get_liked_tweets(id=USER_ID, max_results=5)
for tweet in liked_tweets.data:
    print(tweet.text)

```

Modified to load token from a json file:
```python
with open('token.json') as f:
    data = json.load(f)
    bearer_token = data["bearer_token"]
client = tweepy.Client(bearer_token=bearer_token)
# Get the user's liked tweets (used as a workaround for bookmarks)
user_id = "YOUR_USER_ID"
liked_tweets = client.get_liked_tweets(id=user_id, max_results=5)

for tweet in liked_tweets.data:
    print(tweet.text)
```

Get user ID:
```python
with open('token.json') as f:
    data = json.load(f)
    bearer_token = data["bearer_token"]
client = tweepy.Client(bearer_token=bearer_token)
user = client.get_user(username="your_handle")
user_id = user.data.id
print(f"Your user ID is: {user_id}")
```

I'm getting `556830159`. Hmm I have not updated 'your_handle' though.
- turns out there is a twitter handle called 'your_handle': https://x.com/your_handle
Ok it changed to `354040563` after I updated it to jwt0625.

2024-09-28T21:52:00-07:00
Seems like I need to fix the authentication:
`pip install authlib`

