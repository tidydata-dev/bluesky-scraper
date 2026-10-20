# Bluesky Scraper — Python, Node.js & cURL examples

Get **Bluesky** data without an account: **post search** (keyword, #hashtag, language, dates), **profiles and their posts**, **replies to a post**, **followers / following** and **user search** — with likes, reposts, replies, quotes, links, images and follower counts. **$0.50 per 1,000 results**.

Hosted on Apify: **[Bluesky Scraper](https://apify.com/tidydata/bluesky-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/bluesky-scraper").call(run_input={
    "searchTerms": [
        "#photography"
    ],
    "language": "en",
    "maxItems": 200,
    "addAuthorStats": True
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['authorHandle', 'likeCount', 'text', 'url']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/bluesky-scraper').call({"followersOf": ["bsky.app"], "maxItems": 300});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~bluesky-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"followersOf": ["bsky.app"], "maxItems": 300}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
