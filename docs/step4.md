---
id: step4
title: Step 4 - Simple meta
---

We call meta a single catalog entry. In our case this will be a movie. The meta provides information for the user. Some of the most notable details are, for example, the title of the movie, it's director and cast, poster, logo and so on. Here we will describe one example movie to give you an idea of how this works.

In most cases Stremio's internal add-on Cinemeta will handle the metadata for us, as long as we use IMDB IDs for our items. Feel free to [skip this step](/stremio-addon-guide/step5) if your add-on doesn't need custom meta.

Importance of metadata
---

Stremio provides the Cinemeta add-on, because of the importance of metadata. If we want superb user experience, we need to provide a high quality data. Cinemeta provides metadata by **IMDB ID**.

What if our video is not featured in IMDB? It may be an indie movie or authored by you video, uploaded somewhere. In cases like this we provide a way for your add-on to describe the content it provides.

Update the manifest
---

We need to tell Stremio that our add-on provides additional metadata for otherwise unknown content.

```JavaScript
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    }
]
```

Here we introduce a new way of describing a resource - with an object. This object provides not only the `name` of the resource, but also what `types` of content it will describe and also a prefix filter. We call the prefix filter `idPrefixes`. In it's essence it is an array of prefixes, that Stremio checks against to decide whether to ask our add-on for data or not.

Here is how it looks our manifest now:

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
            "types": ["movie"],
            "idPrefixes": ["hiwrld_"]
        }
    ],
    "types": ["movie"],
    "catalogs": [
        {"id": "movieCatalog", "type": "movie", "name": "Hello, Movies"}
    ]
}
```

Apart from the catalog, every other resource in Stremio makes use of IDs and therefore may be filtered by `idPrefixes`.

In our case we stated that any ID that starts with `hiwrld_` may be handled by our add-on. In case that our add-on is unable to provide data for this ID, Stremio will ask the next one that matches the requested ID.

Challenge Stremio
---

Now we stated that we will handle meta items which ID starts wit `hiwrld_`, but our add-on does not provide an item with such ID. So let's create one.

In the previous step we've created a catalog with two movies. All of them indexed in IMDB. Now we are going to update our catalog with one more meta item, that happens to be a movie, not featured in IMDB - [a jellyfish video](http://jell.yfish.us/).

Let's create a catalog entry for our video. Append the following listing in the `metas` array of our catalog.

```json
{
    "id": "hiwrld_jellyfish",
    "type": "movie",
    "name": "Jellyfish",
    "poster": "https://images.unsplash.com/photo-1496108493338-3b30de66f9be",
    "genres": ["Demo", "Nature"]
}
```

Our catalog should look like this now:

```json
{
    "metas": [
        { "type": "movie", "id": "tt0032138", "name": "The Wizard of Oz", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Adventure", "Family", "Fantasy", "Musical"] },
        { "type": "movie", "id": "tt0017136", "name": "Metropolis", "poster": "https://images.metahub.space/poster/medium/tt0017136/img", "genres": ["Drama", "Sci-Fi"] },
        {
            "id": "hiwrld_jellyfish",
            "type": "movie",
            "name": "Jellyfish",
            "poster": "https://images.unsplash.com/photo-1496108493338-3b30de66f9be",
            "genres": ["Demo", "Nature"]
        }
    ]
}
```

Provide metadata
---

If our add-on matches the type and prefix, Stremio will ask it for metadata. The route's location would be `/meta/<type>/<meta id>.json`. As of now, we provide only the `movie` type.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```bash
mkdir -p meta/movie
```
<!--cmd-->
```batch
mkdir meta\movie
```
<!--END_DOCUSAURUS_CODE_TABS-->

We have the path ready, so now we can create our `meta/movie/hiwrld_jellyfish.json` containing the video metadata we want to show. As we said in the previous step, the catalog entries are meta preview, so we will build upon it.

```json
{
    "meta": {
        "id": "hiwrld_jellyfish",
        "type": "movie",
        "name": "Jellyfish",
        "poster": "https://images.unsplash.com/photo-1496108493338-3b30de66f9be",
        "genres": ["Demo", "Nature"],
        "description": "A .mkv video clip useful for testing the network streaming and playback performance of media streamers & HTPCs.",
        "cast": ["Some random jellyfishes"],
        "director": ["ScottAllyn"],
        "logo": "https://b.kisscc0.com/20180705/yee/kisscc0-art-forms-in-nature-jellyfish-recapitulation-theor-jellyfish-5b3dcabcb00692.802484341530776252721.png",
        "background": "https://images.unsplash.com/photo-1461783470466-185038239ee3",
        "runtime": "30 sec"
    }
}
```

As you can see, we have added a lot more data to our meta info. You can check all the fields available in the [API documentation](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/meta.md).

Test the meta
---

You can now reinstall the add-on again, as described in step 2. You will see the new Jellyfish video in our catalog. When you click it, Stremio will request the meta resource from our add-on. This can be seen in the log of our HTTP server. Now all the information we defined above will be displayed in front of us.

Summary
---

If you check our add-on now, you'll notice the new jellyfish movie, with pretty pictures and nice description. In the next step we will provide some streams so you can watch the movies.

