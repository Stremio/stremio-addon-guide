---
title: Deploying
---

> **Note:** Although deploying is recommended, there is also the alternative of using [localtunnel](https://github.com/localtunnel/localtunnel) to host your add-ons locally.

Stremio add-ons require hosting in order to be published.

If you've built a great add-on, and need help with hosting your add-on, you are welcome to contact us at [addons@stremio.com](addons@stremio.com)

### Publishing to Stremio

If you want your add-on to appear in the list of Community add-ons in Stremio, you can [announce it here](https://stremio.github.io/stremio-publish-addon/index.html).

You can do that even from the command line:

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```sh
curl -d '{"transportUrl": "http://example.com/manifest.json", "transportName": "http"}' -w '\n' -H 'Content-Type: application/json' -X POST 'https://api.strem.io/api/addonPublish'
```
<!--PowerShell-->
```PowerShell
Invoke-RestMethod -Body (@{transportUrl="http://example.com/manifest.json"; transportName="http"} | ConvertTo-Json) -Method 'Post' -ContentType "application/json" -Uri "https://api.strem.io/api/addonPublish"
```
<!--END_DOCUSAURUS_CODE_TABS-->

You just need to substitute the value of the `transportURL` with the location where your add-on is hosted.
