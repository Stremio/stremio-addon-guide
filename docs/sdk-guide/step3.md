---
title: 3. Meta
---

We call meta a single catalog entry. In this particular case this will be a movie. The meta provides information for the user. Some of the most notable details are, for example, the title of the movie, its director and cast, poster, logo and so on.

The catalog items are called meta previews. A meta item builds upon a meta preview by adding additional information.

One very important property of the meta item is an array of videos. Every video element consists of at least `id` and `title`. The `streams` in Stremio are mapped by the video ID. In the case of movies, the videos array is optional and if it is available, it should contain only one video which ID is the same as the meta item's ID.

In most cases, Stremio's internal add-on Cinemeta will handle the metadata for us. It provides meta items indexed by IMDB ID. The video IDs for the TV shows are defined as `{imdb id}:{season}:{episode}`. Generally, your add-on will provide streams for movies and TV shows already handled by the Cinemeta add-on. Feel free to [skip this step](/stremio-addon-guide/sdk-guide/step4) if your add-on doesn't need custom meta.

## Update the manifest

You need to tell Stremio that your add-on provides additional metadata for otherwise unknown content.

```js
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    }
]
```

Here we introduce a new way of describing a resource - with an object. This object provides not only the `name` of the resource, but also what `types` of content it will describe and also a prefix filter. We call the prefix filter `idPrefixes`. In its essence it is an array of prefixes, that Stremio checks against to decide whether to ask your add-on for data or not.

Here is how it looks in your manifest now:

```js
// Docs: https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/manifest.md
const manifest = {
    "id": "community.helloworld",
    "version": "0.0.1",
    "catalogs": [
        {
            "type": "movie",
            "id": "top"
        }
    ],
    "resources": [
        "catalog",
        {
            "name": "meta",
            "types": ["movie"],
            "idPrefixes": ["hiwrld_"]
        }
    ],
    "types": [
        "movie",
        "series"
    ],
    "name": "he",
    "description": "f"
}
```

Apart from the catalog, every other resource in Stremio makes use of IDs and therefore may be filtered by `idPrefixes`.

In this particular case we stated that any ID that starts with `hiwrld_` may be handled by your add-on. In case the add-on is unable to provide data for this ID, Stremio will ask the next one that matches the requested ID.

## Challenge Stremio

Now we stated that we will handle meta items which ID starts wit `hiwrld_`, but the add-on does not provide an item with such ID. So let's create one.

In the previous step you've created a catalog with two movies. All of them are indexed in IMDB. Now you need to update your catalog with one more meta item that happens to be a movie not featured in IMDB - [a jellyfish video](http://jell.yfish.us/).

Let's create a catalog entry for this video. Append the following listing to your catalog array inside the `getMoviesCatalog` function:

```js
{
    id: "hiwrld_jellyfish",
    type: "movie",
    name: "Jellyfish",
    poster: "https://images.unsplash.com/photo-1496108493338-3b30de66f9be",
    genres: ["Demo", "Nature"]
}
```

## Provide metadata

Now is the time to implement the meta handler. We will use similar approach as with the catalog:

```js
function getMovieMeta(id) {
    const metas = {
        hiwrld_jellyfish: {
            id: "hiwrld_jellyfish",
            type: "movie",
            name: "Jellyfish",
            poster: "https://images.unsplash.com/photo-1496108493338-3b30de66f9be",
            genres: ["Demo", "Nature"],
            description: "A .mkv video clip useful for testing the network streaming and playback performance of media streamers & HTPCs.",
            cast: ["Some random jellyfishes"],
            director: ["ScottAllyn"],
            logo: "https://b.kisscc0.com/20180705/yee/kisscc0-art-forms-in-nature-jellyfish-recapitulation-theor-jellyfish-5b3dcabcb00692.802484341530776252721.png",
            background: "https://images.unsplash.com/photo-1461783470466-185038239ee3",
            runtime: "30 sec"
        },
    }
    Promise.resolve(metas[id] || null)
}

builder.defineMetaHandler(({type, id}) => {
    // Docs: https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/requests/defineMetaHandler.md
    let results;

    switch(type) {
        case 'movie':
            results = getMovieMeta(id)
            break
       default:
            results = null
            break
    }
    return Promise.resolve({ meta: results })
})
```

As you can see, we have added a lot more data to the meta info. You can check all the fields available in the [API documentation](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/meta.md).
