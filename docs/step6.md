---
title: 6. Multiple videos
---

In your catalog you only have movies - the simplest form of media Stremio can present.

Now you will be dealing with TV shows. The difference is minimal - unlike the TV shows, a movie has only one video.

Update the manifest
---

TV shows are meant to be presented under the `series` type. You need to update the manifest to indicate that you support this type of metadata along with the `movie` type. You will state that in several places in the manifest - the `types` and `catalogs` arrays and also the types in the meta resource.

Here is how the `manifest.json` looks now:

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
        {"id": "movieCatalog", "type": "movie", "name": "Hello, Movies"},
        {"id": "seriesCatalog", "type": "series", "name": "Hello, TV Shows"}
    ]
}
```

Now you have "series" listed in the both `types` arrays and also you have one new catalog called "Hello, TV Shows". Let's populate that catalog. You will add only two shows. This will be enough for demonstration purposes.

Populate the catalog
---

First you need to create a directory for your `series` catalog:

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```sh
mkdir catalog/series
```
<!--cmd-->
```batch
mkdir catalog\series
```
<!--END_DOCUSAURUS_CODE_TABS-->

Now you will create the series catalog `catalog/series/seriesCatalog.json`:

```json
{
    "metas": [
        { "type": "series", "id": "tt1748166", "name": "Pioneer One", "poster": "https://images.metahub.space/poster/medium/tt1748166/img", "genres": ["Drama", "Sci-Fi"]},
        { "type": "series", "id": "hiwrld_tt0147753", "name": "Captain Z-Ro", "poster": "https://www.captain-z-ro.com/images/FLYER-PAGE-1_250.gif", "genres": ["Sci-Fi", "Children", "Educational"] }
    ]
}
```

As you can see, there is nothing new here. Everything is just like in the example with the movies catalog.

> Every catalog is structured the same, no matter what type of resource it represents.

Provide meta
---

Now is the time to provide meta. Let the `Cinemeta` add-on to take care about the first show in your catalog. It has a valid IMDB ID that Cinemeta is aware of.

For the second show you will provide your own meta.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```sh
mkdir meta/series
```
<!--cmd-->
```batch
mkdir meta\series
```
<!--END_DOCUSAURUS_CODE_TABS-->

The contents of your meta item is `meta/series/hiwrld_tt0147753.json`:

```json
{
    "meta": {
            "id": "hiwrld_tt0147753",
            "type": "series",
            "name": "Captain Z-Ro",
            "description": "From his secret laboratory, Captain Z-Ro and his associates use their time machine, the ZX-99, to learn from the past and plan for the future.",
            "releaseInfo": "1955-1956",
            "logo": "https://fanart.tv/fanart/tv/70358/hdtvlogo/captain-z-ro-530995d5e979d.png",
            "imdbRating": 6.9,
            "poster": "https://www.captain-z-ro.com/images/FLYER-PAGE-1_250.gif",
            "background": "https://www.captain-z-ro.com/images/Captain-Z--R0_500.jpg",
            "genres": ["Sci-Fi", "Children", "Educational"],
            "runtime": "15 mins.",
            "videos": [
                { "season": 1, "episode": 1, "id": "hiwrld_tt0147753:1:1", "title": "Christopher Columbus", "released": "1955-12-18" },
                { "season": 1, "episode": 2, "id": "hiwrld_tt0147753:1:2", "title": "Daniel Boone", "released": "1955-12-25" }
            ]
    }
}
```

Just like the movie's meta, here you build upon the meta preview returned by the catalog.

Unlike the movie meta, here you have a videos array that provides video objects with information about every episode. You can learn more about the video objects in the [API documentation](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/meta.md#video-object).

Checking the progress
---

At this point you have provided enough data to be able to test the result.

Reinstall your add-on and see if your new catalog for series is available at the bottom of Stremio's board.

There you should see a section called "Series - Hello, TV Shows" containing the two shows from your catalog. If you open any of them, detailed metadata should be revealed with the episodes listed on the right side.

![The series catalog](/stremio-addon-guide/img/catalog-series.png)

Add streams
---

Again, you need to create a directory for your `series` streams:

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```sh
mkdir stream/series
```
<!--cmd-->
```batch
mkdir stream\series
```
<!--END_DOCUSAURUS_CODE_TABS-->

Now let's provide data for the first stream `stream/series/hiwrld_tt0147753:1:1.json`:

```json
{
    "streams": [
        { "title": "YouTube", "ytId": "5EQw5NYlbyE" }
    ]
}
```

... and the second one `stream/series/hiwrld_tt0147753:1:2.json`:

```json
{
    "streams": [
        { "title": "YouTube", "ytId": "ZzdBKcVzx9Y" }
    ]
}
```

The most notable thing here is not the stream objects but the resource location. As you can see the first stream's ID is `hiwrld_tt0147753:1:1` and the second one - `hiwrld_tt0147753:1:2`.

> The stream IDs are the same as the IDs of the video objects returned by the `meta` resource. In our movies example there is no video object so it is generated implicit one with the same ID as the meta item ID.

Test the result
---

We are ready with this step. You can now test the progress of your add-on again. Reinstall it and check if the episodes of the second show are playable. If for some reason they are not, check the logs of the HTTP server. It could be a typo in the route or error in a JSON object.

Summary
---

Here we extended almost any aspect of your add-on. We covered the case with multiple videos and how it affects the other parts of the add-on. You can now try to implement a `channel` catalog on your own. It is very similar to how series work.
