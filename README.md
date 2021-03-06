# Deploy Spy: Deployment Tracking Chrome Extension
If you are using the [Github's service hook](http://www.pivotaltracker.com/community/tracker-blog/guide-githubs-service-hook-tracker)
for [Pivotal Tracker](http://www.pivotaltracker.com) to label git commits with Tracker stories, then you can use
this extension to help you figure out exactly where your Tracker stories are deployed!

In order to use this plugin, you will need to provide a specifically formatted JSON file for each environment your app can be deployed to. For example, let's say you have three environments: **staging, beta, and test**. When you deploy new code to each of these environments, you'll also deploy the JSON file containing a map of Pivotal Tracker story ids to commit SHAs. Deploy Spy will be configured with a list of URLs for each environment so that it can find these files. Then, when you open a story in Pivotal Tracker, you'll see a section in the details showing what environment this story is deployed to.
# Installation

Get Deploy Spy from the Chrome store, or if you want the local/dev
version:

* Clone this repo locally
* Open the Chrome kebab menu and go to More Tools > Extensions
* Ensure "Developer mode" toggle is turned on
* Click "Load Unpacked" and select the location that this repo was cloned to in the open dialog

# Extension configuration

Deploy Spy can be configured via the "Options" link on the Chrome Extension entry.

Configure the extension with these three environments like so:

| Environment   | Destination URL**                        | Commits URL**                            |
| ------------- |------------------------------------------|------------------------------------------|
| beta          | https://beta.example.com/                | https://beta.example.com/commits.json    |
| staging       | https://staging.example.com/             | https://staging.example.com/commits.json |
| testing       |                                          | https://test.example.com/commits.json    |

_**Note: The commit URLs must be served via HTTPS._
_**Note: Omit destination URLs to refrain from creating a clickable link._

Alternately, you can import a `deploy_spy_config.json` from
a known hosted location, which
is a json object containing environments and URLs, e.g.:

```
{
  "beta": {
    "href": "https://beta.example.com/",
    "url": "https://beta.example.com/commits.json"
  },
  "staging": {
    "href": "https://staging.example.com/",
    "url": "https://staging.example.com/commits.json"
  }
}
```

## Commits JSON format

Then either manually, as part of your deployment scripts, etc. update the commits.json file with recent commit
data.  For example, generate data for all the commits within the last 30 days.  [This sample
rake task](sample.rake) is close to what Tracker is using to generate this data.

The data should be valid JSON with a format like this:

```json
"pivotal tracker story id": [
  "first git commit sha",
  "second git commit sha",
  "... and so on"
],
"123456": [
  "987s6afdsg8796dsf78h9hfa857s9h",
  "asdfg6asdf7ga7sfhgas5hgf5asdfg"
]
```

That's it!

Once configured correctly, the extension will insert data about the deployment environments when you expand a story. Specifically, it will show the aggregate of all environments any part of the story is deployed to under the story info box:
![Story Detail](https://github.com/pivotaltracker/tracker_deployment_chrome_extension/blob/master/story_detail.png "Story Detail")
