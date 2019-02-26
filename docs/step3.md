---
id: step3
title: Step 3 - Simple meta
---

In most cases Stremio's internal add-on Cinemeta will handle the metadata for us, as long as we use IMDB IDs for our items. Feel free to [skip this step](./step4) if your add-on doesn't need custom meta.


```json
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    }
]
```

