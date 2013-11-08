Every Azure web site has an associated Kudu service site.

_**Update**: actually, sites that were created way back (say early 2013 or before) may not have a Kudu site. To create it, choose 'Set up deployment from source control', and then 'Local git'. Then you can instantly 'Delete git repository', and you will be left with the Kudu service working._

If your web site has URL `http://mysite.azurewebsites.net/`, then the root URL of the Kudu service is `https://mysite.scm.azurewebsites.net/`. Note the added `scm` token.

For mobile services, the site URL looks like `https://myservice.scm.azure-mobile.net/`, and the Kudu root URL is 
`https://myservice.scm.azure-mobile.net/default.aspx` (**note the explicit /default.aspx necessary for mobile services**).

Notes:

- The Kudu service should always be accessed over https and not http to protect sensitive data.
- The Kudu service host name is not affected by the presence of custom host names in your site. It always has the pattern above.
- The Kudu service will prompt you for credentials. Here, you can use either the user publishing credentials (same as you use for git), or the site level credentials. See [[Deployment-credentials]] for details.

## Simple trick to avoid manually typing credentials

Notes:

- This trick only works for sites that have git/hg/dropbox publishing enabled
- It doesn't work in IE

Here are the steps:

- In the Azure portal, go to the Configure tab, and look for 'DEPLOYMENT TRIGGER URL'. You'll see a URL that looks like this: `https://$mysite:37jbd3ejoQYAHa1bwufqpeFoxGNuF4ZohhjJcPLelsRll4tCyJe0c5Sv2GJv@mysite.scm.azurewebsites.net/deploy`. Note that it has the site-level publishing credentials embedded, which is what makes it work without prompting for credentials.
- Remove the /deploy at the end, and paste the rest in your browser's address bar. e.g. `https://$mysite:37jbd3ejoQYAHa1bwufqpeFoxGNuF4ZohhjJcPLelsRll4tCyJe0c5Sv2GJv@mysite.scm.azurewebsites.net`. You're now looking at the root of the Kudu service.

## Once you're in the Kudu service

The root of the Kudu service has links to various things that can be done from there, and are discussed elsewhere in this wiki. e.g., you can:

- Get the Kudu diagnostic dump
- Launch the [[diagnostic console|Kudu-console]]
- See info about the runtime environment
- Navigate the REST API