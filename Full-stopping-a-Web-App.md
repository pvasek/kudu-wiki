All Web Apps have both a main site and an SCM site (where Kudu runs). When you click Stop in the portal, only the main site is stopped, while the SCM site keeps running. Stopping both is possible, but the steps are more complex.

To stop both the main site and SCM site:

- Go to Resource Explorer (https://resources.azure.com/)
- Find your Web App (aka 'site'), either from the tree or using search box
- While on your site node, you'll see some JSON in the right panel. Click `Edit` at the top (you may have to switch to Read/Write mode first)
- In the JSON, you need to change two things: change `"state"` from `"Running"` to `"Stopped"`, and set `"scmSiteAlsoStopped"` to `true`
- Press the green `PUT` button at the top