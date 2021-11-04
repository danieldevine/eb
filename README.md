# Elephant Bird - DEPRECTATED

**Deprecated as of Nov 2021 - please use Bird Elephant instead - its much improved!**
This will recieve security patches only from now on.
---

**Note**: This package currently caters for **bearer token based app access only**. [I'm @coderjerk on Twitter if you want to discuss.](https://twitter.com/coderjerk)

These endpoints are early access so subject to change. This package does not support old v1.1 endpoints.

#### Currently supported:

-   API v2
    -   Tweets
        -   Recent Search
        -   Lookup
        -   Filtered Stream (basic support)
        -   Timeline
    -   Users
        -   Follows Lookup
        -   User Lookup


To use this package you must have an approved developer account, and have activated the new developer portal.

Learn more about getting access to the Twitter API v2 endpoints:

[Twitter Getting Started Docs](https://developer.twitter.com/en/docs/twitter-api/getting-started/guide)

### API Reference

[Twitter Recent Search Endpoint API Reference](https://developer.twitter.com/en/docs/twitter-api/tweets/search/api-reference/get-tweets-search-recent)

[Lookup Multiple Tweets API Reference](https://developer.twitter.com/en/docs/twitter-api/tweets/lookup/api-reference/get-tweets)

[Lookup Single Tweets API Reference](https://developer.twitter.com/en/docs/twitter-api/tweets/lookup/api-reference/get-tweets-id)

Note that operator support is quite sparse at the moment which makes the use of tweets and media more than a little risky in some contexts - for example filtering NSFW content is not yet possible. I don't know if this is in Twitter's plans or not.

## Install:

Install via composer.

```bash
$ composer require coderjerk/elephant-bird
```

## Auth

Bearer token support only for now. Copy the contents of .env.example to .env in your project and populate with your own credentials that you have set up for your project in the Twitter dev portal. If you aren't using .env in your project, you will need to set it up, [details here](https://github.com/vlucas/phpdotenv)

## Examples:


#### Recent Search

Search the 14 most recent tweets relating to football.

```php
use Coderjerk\ElephantBird\RecentSearch;

$params = [
    'query'        => 'football',
    'max_results'  => 14,
];

$search = new RecentSearch;
$result = $search->RecentSearchRequest($params);

$tweets = $result->data;

```

Search for media:

```php
use Coderjerk\ElephantBird\RecentSearch;

$params = [
    'query' => 'dancing has:images ',
    'tweet.fields' => 'attachments,author_id,created_at',
    'expansions'   => 'attachments.media_keys',
    'media.fields' => 'public_metrics,type,url,width',
    'max_results'  => 10,
];

$search = new RecentSearch;
$result = $search->RecentSearchRequest($params);
$media = $result->includes->media;

```

#### Tweet Lookup

Lookup details about multiple tweets by Id - if a single id is provided Elephant Bird will choose the single tweet endpoint:

```php
use Coderjerk\ElephantBird\TweetLookup;

$ids = [
    '1261326399320715264',
    '1278347468690915330'
];

$params = [
    'tweet.fields' => 'attachments,author_id,created_at,public_metrics,source'
];

$lookup = new TweetLookup;
$tweets = $lookup->getTweetsById($ids, $params);
```
#### Timeline

Get a given user's Tweets.

```php
use Coderjerk\ElephantBird\TimeLine;

$timeline = new TimeLine;

$params = [
    'tweet.fields' => 'attachments,author_id,created_at,public_metrics,source'
];

$tweets = $timeline->getTweets('802448659', $params);

```

Get a given user's mentions.

```php
use Coderjerk\ElephantBird\TimeLine;

$timeline = new TimeLine;

$params = [
    'tweet.fields' => 'attachments,author_id,created_at,public_metrics,source'
];

$mentions = $timeline->getMentions('802448659', $params);

```

#### User Lookup

Lookup a single user by username:

```php
use Coderjerk\ElephantBird\UserLookup;

$params = [
    'user.fields' => 'id'
];

$usernames = [
    'coderjerk'
];

$userLookup = new UserLookup;
$user = $userLookup->lookupUsersByUsername($usernames, $params);

```

Lookup multiple users by id:

```php
use Coderjerk\ElephantBird\UserLookup;

$ids = [
    '802448659',
    '16298441'
];

$userLookup = new UserLookup;
$user = $userLookup->lookupUsersById($ids, $params);
```
#### Follows Lookup

Get Followers

```php
use Coderjerk\ElephantBird\FollowsLookup;

$follows = new FollowsLookup;

$params = [
    'tweet.fields' => 'attachments,author_id,created_at,public_metrics,source'
];

$followers = $follows->getFollowers('802448659', $params);

```

Get Following

```php
use Coderjerk\ElephantBird\FollowsLookup;


$follows = new FollowsLookup;

$params = [
    'tweet.fields' => 'attachments,author_id,created_at,public_metrics,source'
];

$following = $follows->getFollowing('802448659', $params);
```

## Contributing

Fork/download the code and run
`composer install`

This package is in the early stages of development. Issues, pull requests and other contributions most welcome.

You can [look at the project board here for upcoming features:](https://github.com/danieldevine/elephant-bird/projects/1)
