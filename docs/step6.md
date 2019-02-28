---
id: step6
title: Strep 6 - Multiple videos
---

In our catalog we have only movies - the simplest form of media Srtemio can present.

Now we will be dealing with TV shows. In essence the difference is minimal - unlike the TV shows, a movie has only one video.

Update the manifest
---

TV shows are meant to be presented under the `series` type. We need to update the manifest to indicate that we support this type of metadata along with the `movie` type. We will state that in several places in the manifest - the `types` and `catalogs` arrays and also the types in the meta object.

```json
{
    "id": "my.first.stremio.add-on",
    "version": "1.0.0",
    "name": "Hello, World",
    "description" : "My first Stremio add-on",
    "logo": "https://www.stremio.com/website/stremio-logo-small.png",
    "resources": [
        "catalog",
        {
            "name": "meta",
            "types": ["movie", "series"],
            "idPrefixes": ["hiwrld_"]
        },
        "stream"
    ],
    "types": ["movie", "series"],
    "catalogs": [
        {"id": "movieCatalog", "type": "movie", "name": "Hello, Movies"}
    ]
}
```
