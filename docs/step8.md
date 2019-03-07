---
title: Hosting and publishng
---

@TODO: Describe hosting and publishing options

Publish via PowerShell:

```PowerShell
Invoke-RestMethod -Method 'Post' -ContentType "application/json" -Uri "https://api.strem.io/api/addonPublish 🔗" -Body (@{transportName="http"; transportUrl="http://example.com/manifest.json"} | ConvertTo-Json)

Invoke-RestMethod -Method 'Post' -ContentType "application/json" -Uri "https://api.strem.io/api/addonPublish 🔗" -Body '{"transportName"="http", "transportUrl"="http://example.com/manifest.json"}'
```
