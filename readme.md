# Serverless Reading System

This is the Serlverless FCS working on Webtask.

## Preperations

1. GitHub repository for collecting reading process with issues. 
2. GitHub API token.
3. ZenHub API token.
4. Make sure you've already created at least one milestone and release for your repo.

## Build

### WebTask

Replace the `const` with your information in `reading.js`:

- const REPO_OWNER 
- const REPO_NAME 
- const REPO_ID 
- const REALEASE_ID

[Install Webtask CLI tools](https://webtask.io/cli) on your machine, then build your Function with:

```
wt create index --bundle --secret GITHUB_ACCESS_TOKEN=<your GitHub api token here> --secret ZENHUB_ACCESS_TOKEN=<your zenhub api token here>
```

You would get an interface like:

```
https://wt-<taskid>-0.run.webtask.io/serverless-reading
```

You can run `wt edit index` to open the web editor of your module or to check the logs when it's called.

### IFTTT

There are 3 workflows you need for this service:

- [Add post into Instapaper](https://ifttt.com/applets/70976742d-if-new-item-saved-then-create-a-new-issue) - Create a new issue in Reading repo.
- [Make comments when reading](https://ifttt.com/applets/71054636d-if-new-comment-then-make-a-web-request-to-comment-on-corresponding-issue) - Add a comment under corresponding issue.
- [Archive post after reading](https://ifttt.com/applets/70982991d-if-new-archived-item-then-make-get-webtask-to-close-issue) - Close this issue.

You need to change the workflow configuration with your own information.

## Read and Enjoy!