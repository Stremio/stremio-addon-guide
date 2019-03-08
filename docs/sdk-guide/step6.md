---
title: Deploying
---

> **Note:** Although deploying is recommended, there is also the alternative of using [localtunnel](https://github.com/localtunnel/localtunnel) to host your add-ons locally.

Stremio add-ons require hosting in order to be published. You will need a NodeJS hosting solution, as Stremio Add-ons made with the Stremio Add-on SDK are NodeJS apps.

We recommend:

- [Now.sh](https://zeit.co/) - [free with some restrictions](https://zeit.co/pricing)
- [Heroku](https://www.heroku.com) - [free with some restrictions](https://www.heroku.com/pricing)
- [cloudno.de](https://cloudno.de) - [free for up to 150k requests/month](https://cloudno.de/pricing)
- [Evennode](https://www.evennode.com) - [free for 7 days trial](https://www.evennode.com/pricing)

We hugely recomment using `Now.sh`, as it is extremely easy to use.

You can also check this very comprehensive [guide by nodejs](https://github.com/nodejs/node-v0.x-archive/wiki/node-hosting).

Stremio add-ons are deployed just like regular nodejs apps, so follow the nodejs instructions provided by your particular service provider.

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
