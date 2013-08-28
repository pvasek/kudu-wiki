Every Azure web site has an associated Kudu service site.

If your web site has URL `http://mysite.azurewebsites.net/`, then the root URL of the Kudu service is `https://mysite.scm.azurewebsites.net/`. Note the added `scm` token.

Notes:

- The Kudu service should always be accessed over https and not http to protect sensitive data.
- The Kudu service host name is not affected by the presence of custom host names in your site. It always has the pattern above.
- The Kudu service will prompt you for credentials. Here, you can use either the user publishing credentials (same as you use for git), or the site level credentials. See [[Deployment-credentials]] for details.

## Simple trick to avoid manually typing credentials

Notes:

- This trick only works for sites that have git/hg/dropbox publishing enabled
- It works in Chrome but not in IE. Not sure about other browsers.

Here are the steps:

- In the Azure portal, go to the Configure tab, and look for 'DEPLOYMENT TRIGGER URL'. You'll see a URL that looks like this: `https://$mysite:37jbd3ejoQYAHa1bwufqpeFoxGNuF4ZohhjJcPLelsRll4tCyJe0c5Sv2GJv@mysite.scm.azurewebsites.net/deploy`. Note that it has the site-level publishing credentials embedded.
- Remove the /deploy at the end, and paste the rest in your browser's address bar. You're now looking at the root of the Kudu service.

## Once you're in the Kudu service

The root of the Kudu service has links to various things that can be done from there, and are discussed elsewhere in this wiki. e.g., you can:

- Get the Kudu diagnostic dump
- Launch the diagnostic console
- See info about the runtime environment
- Navigate the REST API