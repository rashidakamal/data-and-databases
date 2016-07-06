# Class 13 - Twitter API

+ Using the Twitter API
    + Searching Twitter using Python
    + Reading the timeline of a particular user
    + Twitter's rate limiting rules

**Next class:** posting to Twitter/bots!

## The Twitter API

[Developer Homepage](https://dev.twitter.com/)

We're interested in the [REST APIs](https://dev.twitter.com/rest/public). We're not going into exactly what REST means, for now know that it basically means that the API has individual resources that can be accessed by URL and it returns JSON.

>> Once upon a time, everyone used XML-RPC (remote prodecure call) -- that's what was used prior to Rest APIs.

Most things that you'd want to do on Twitter, you can do through the Twitter API.

First, in order to use the Twitter API, we need to get authorization. Usually, when you're signed in, you are making requests to the API as the user you're signed in as. We need to do something similar for accessing the Twitter API. Twitter requires OAuth to manage this process -- it's a bit clunky, so we're going to be using a Python library to help us out with that.

There four values have to be included in every request to the Twitter API:
+ consumer key
+ consumer secret
+ access token
+ token secret

We need to start a Twitter application in order to get a **consumer key** and **consumer secret**. Visit [apps.twitter.com](apps.twitter.com) -> click the "Create New App" button. Fill in the required questions (the description doesn't really matter because our app will not be user-facing and the website does not matter, but you do need to provide one).

This authentication process involves the user getting permissions for the app, which then requests the API. The API returns information back to the app, which then returns that data to the user.

The "Keys and Access Tokens" tab on the Application Management page shows you the **consumer key** and the **consumer secret** associated with your application.

Every user that accesses your application will need to acquire their own **access token** and **access secret**. Under the "Keys and Access Tokens" tab, use the "Generate Access Token and Token Secret" to generate those keys. These keys allow you to make API requests on your account's behalf.

We're going to be using a Python library called **twython** to make our API requests easier. Another good one is **tweepy**.

[Documentation for Twython](https://twython.readthedocs.io/en/latest/api.html) -- Twython has a function for every single endpoint in the Twitter API. But sometimes, twython has a slightly different name for a function than what it corresponds to in the Twitter API (which is why it's so important to read the documentation!).
