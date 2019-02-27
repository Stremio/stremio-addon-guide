---
id: step4
title: Step 4 - Providing streams
---

Our add-on now features catalog with item details. Now is the time to supply and some streams.

Update the manifest
---

For streams to work we just need to append `streams` to our `resources` array. That's it.

The resources section of the manifest will now look like that:

```JavaScript
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    },
    "stream"
]
```

Add streams
---

The `stream` resource is reached in similar way to `meta`. There is one difference though. The `id` of the stream corresponds to the video `id` and not the meta item `id`. The reason for this is that some items like `series` and `channlel` are designed to contain more than one video. Fortunately in simple cases like ours, where there are no videos, Stremio is looking for streams that match the meta `id`.

