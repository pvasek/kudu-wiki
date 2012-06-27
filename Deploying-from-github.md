Normally, the way to deploy changes to Kudu is to push to the repository provided by Kudu. As an alternative, some users prefer to push to GitHub, and have that automatically trigger a deployment.

GitHub has a Service Hooks feature which makes implementing this relatively straightforward. Here is the high level workflow:

## Initial setup

- Kudu provides a URL that can be hit to trigger deployments
- The user sets that URL as a github WebHook


## Deployment cycle

- The user pushed a change to GitHub
- GitHub sends a POST request to the URL set as the WebHook, passing it a JSON payload as described in the [GitHub docs](https://help.github.com/articles/post-receive-hooks)
- Kudu receives that request and gets the GitHub repo URL from the payload
- Kudu pulls from the GitHub repo URL (or clones if it doesn't have a repo yet)
- Kudu makes a deployment, using the same logic as if the user had pushed directly to the Kudu repo
