# Serverless Reading System

This is the Serlverless FCS working on Webtask.

## Preperations

1. A new GitHub repository for collecting reading process with issues. (e.g. quentin-chen/reading) 
2. GitHub API token. [apply here](https://github.com/settings/tokens)
3. ZenHub API token. [apply here](https://dashboard.zenhub.io/#/settings)
4. Make sure you've already created at least one milestone and release for your repo with ZenHub.

## Build

Clone this repo and change into repo folder.

### WebTask

Replace the `const` with your information in `routes/reading.js`:

- const REPO_OWNER 
- const REPO_NAME 
- const REPO_ID 
- const REALEASE_ID

[Install Webtask CLI tools](https://webtask.io/cli) on your machine(finished step2), then build your cloud function module with:

```
wt create index --bundle --secret GITHUB_ACCESS_TOKEN=<your GitHub api token here> --secret ZENHUB_ACCESS_TOKEN=<your ZenHub api token here>
```

You would get an interface like:

```
https://wt-<taskid>-0.run.webtask.io/serverless-reading
```

Now your module has alreadu been deployed on Webtask as a service. You can run `wt edit index` to open the online editor of your module or to check the logs when it's called.

### IFTTT

There are 3 workflows you need for this service:

- [Add post into Instapaper](https://ifttt.com/applets/70976742d-if-new-item-saved-then-create-a-new-issue) - Create a new issue in Reading repo.

The other two workflow you need to [create from scrach](https://ifttt.com/create):

- Close issue in Reading repo
    - trigger: archived post in instapaper
    - action: webhook
        - url: `https://wt-<your taskid>-0.run.webtask.io/serverless-reading/reading?title=<<<{{Title}}>>>`
        - method: `GET`
        - content type: `application/json`
- Make comments under issue
    - trigger: comments post in instapaper
    - action: webhook
        - url: `https://wt-<your taskid>-0.run.webtask.io/serverless-reading/reading-note?title=<<<{{Title}}>>>`
        - method: `POST`
        - body: `{ "note": "{{Comment}}\n\n> {{HighlightedText}}" }`

You need to change the workflow configuration with your own information.

### GitHub Webhook

Creat a Webhook in your reading repo settings:

- url: `https://wt-<your taskid>-0.run.webtask.io/serverless-reading`
- event: issue, push

## Read and Enjoy!