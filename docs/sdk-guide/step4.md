---
title: Streams
---

The streams in Stremio's add-ons are just shortcuts to the real media. They does not contain any videos. They just point Stremio to the right place where that videos can be found.

## Update the manifest

If you've selected "streams" in addon-bootstrap, you will have `streams` already listed in the `resources` array.

```
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    },
    "streams"
]
```

## Add streams

At this point we have two videos in our catalog. In odrer to serve them, we will create a function:

```js
function getMovieStreams(id) {
    const streams = {
        tt1254207: [
            { "title": "HTTP location", "yt_ID": "aqz-KE-bpKQ"}
        ]
        hiwrld_jellyfish: [
            { "title": "Web, 3 MBps, HD", "url": "http://jell.yfish.us/media/jellyfish-3-mbps-hd-h264.mkv" },
            { "title": "Web 15 MBps, HD", "url": "http://jell.yfish.us/media/jellyfish-15-mbps-hd-h264.mkv" },
            { "title": "Web, 120 MBps, 4K", "url": "http://jell.yfish.us/media/jellyfish-120-mbps-4k-uhd-h264.mkv" }
        ]
    }
    return Promise.resolve(streams[id] || [])
}
```

Let's handle this resource:

```js
addon.defineStreamHandler(({type, id}) => {
    // Docs: https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/requests/defineStreamHandler.md
    let results;

    switch(type) {
        case 'movie':
            results = getMovieStreams(id)
            break
       default:
            results = Promise.resolve( [] )
            break
    }
    return results.then(streams => ({streams}))
})
```

> When your add-on is unable to provide streams for particular video, you have to return an empty streams array. Otherwise and error is shown to the user.

> As you can see the stream object can point to variety of streams. The complete reference is [located here](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/stream.md).
