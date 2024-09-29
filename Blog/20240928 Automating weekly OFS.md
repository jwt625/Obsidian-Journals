2024-09-28T21:04:17-07:00
# Shortcuts

Dashboard: https://developer.twitter.com/en/portal/dashboard


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

What kind of bullshits... Going to see if I could find examples.
- https://github.com/xdevplatform/Twitter-API-v2-sample-code/tree/main/Bookmarks-lookup

Ok find out I have not setup the user authentication settings:
![[Pasted image 20240928220219.png]]

2024-09-28T22:05:57-07:00
ok done:
![[Pasted image 20240928220600.png]]
- used `http://127.0.0.1:5000/callbac` for `callback URL`
- used `http://example.com` for `website URL`


# Switch to JavaScript

2024-09-28T22:49:48-07:00
Still trying to figure out how the authentication works:
- https://developer.x.com/en/docs/x-api/tweets/bookmarks/api-reference/get-users-id-bookmarks

Maybe I should switch to JavaScript since all the examples are generated in JS...
2024-09-28T22:56:20-07:00
Updated node.js from v18 to v20.

Installing shit in VSCode terminal:
`npm install axios express simple-oauth2 dotenv`

![[Pasted image 20240928225756.png]]

2024-09-28T23:05:31-07:00
Installing twitter api
```
npm install twitter-api-sdk
```
![[Pasted image 20240928230528.png]]

Same error:
![[Pasted image 20240928230610.png]]
What kind of shit is this...
2024-09-28T23:18:34-07:00
Ok changed the API app to web app.
```
https://twitter.com/i/oauth2/authorize?state=my-state&code_challenge_method=s256&client_id=WEtDWnlKbTFqdEo1RWJOSkRpYjA6MTpjaQ&scope=bookmark.read+tweet.read+users.read&response_type=code&redirect_uri=http%3A%2F%2F127.0.0.1%3A3000%2Fcallback&code_challenge=mImjkwyjpRdK4DFVh6Ki-vmbimQh5kGOQopmRW5EIaQ
```
- fuck, same error.
- https://devcommunity.x.com/t/always-getting-a-something-went-wrong-using-oauth2/175507
	- `On Developer portal and your app code the “callback_url” must be identical.`

## Fixed "something went wrong"
![[Pasted image 20240928233003.png]]
Fuck okay it is because of the callback_url not being identical.
Now get the following error in terminal:
```
{
  status: 403,
  statusText: 'Forbidden',
  headers: {
    'api-version': '2.111',
    'cache-control': 'no-cache, no-store, max-age=0',
    'content-disposition': 'attachment; filename=json.json',
    'content-encoding': 'gzip',
    'content-length': '328',
    'content-type': 'application/json; charset=utf-8',
    date: 'Sun, 29 Sep 2024 06:30:23 UTC',
    perf: '7402827104',
    server: 'tsa_p',
    'strict-transport-security': 'max-age=631138519',
    'x-access-level': 'read',
    'x-connection-hash': 'ced81d5e7ea31457e693c0f8571690e04ab58c097483157c6e0583fdfe72d32d',   
    'x-content-type-options': 'nosniff',
    'x-frame-options': 'SAMEORIGIN',
    'x-rate-limit-limit': '40000',
    'x-rate-limit-remaining': '39999',
    'x-rate-limit-reset': '1727592323',
    'x-response-time': '24',
    'x-transaction-id': 'f927dc9a25a07820',
    'x-xss-protection': '0'
  },
  error: {
    client_id: '29394064',
    detail: 'When authenticating requests to the Twitter API v2 endpoints, you must use keys and tokens from a Twitter developer App that is attached to a Project. You can create a project via the developer portal.',
    registration_url: 'https://developer.twitter.com/en/docs/projects/overview',
    title: 'Client Forbidden',
    required_enrollment: 'Appropriate Level of API Access',
    reason: 'client-not-enrolled',
    type: 'https://api.twitter.com/2/problems/client-forbidden'
  }
}
```
![[Pasted image 20240928233104.png]]
- https://devcommunity.x.com/t/when-authenticating-requests-to-the-twitter-api-v2-endpoints-you-must-use-keys-and-tokens-from-a-twitter-developer-app-that-is-attached-to-a-project-you-can-create-a-project-via-the-developer-portal/192346/12

2024-09-28T23:47:36-07:00
Removed "tweet.read" and "users.read" from scopes. Now it just says forbidden. Should add tweet.read back.

2024-09-28T23:58:44-07:00
Found that it is because free API only has post, delete and get user...
![[Pasted image 20240928235933.png]]

Tested posting and it works...
![[Pasted image 20240928235849.png]]
Returning value:
- `{"data":{"id":"1840284954925903891","edit_history_tweet_ids":["1840284954925903891"],"text":"Test API"}}`



