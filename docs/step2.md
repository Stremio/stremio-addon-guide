---
id: step2
title: Step 2 - The catalog
---

We covered the basic structure of the add-on manifest. Now it is time to make our add-on useful.

Let's create a catalog.

## Deciding the types

Our first thing to consider, when creating an add-on catalog is to choose what types of media will be provided.

The type determines the way the content is presented and organized within the app.

`movie` - Presents a single movie. This type doesn't provide any `video` objects. The sources are directly linked to the meta item.

`series` - Used for TV shows and like. The videos are organized in seasons.

`channel` - Channel of videos. It's like series, but the videos are listed in single list.

`tv` - Used for live TV. Similar to movie, but the sources are expected to be live stream without duration.

We can choose one or more types. For this basic example we will use only the movie type.

## Update the manifest

As we are adding more features to our add-on we must update the manifest so Stremio will be able to recognize and use these features.

We need to do several changes.

 * Add `catalog` to the resources array
 * Add `catalogs` array with description of our catalog
 * Add `movie` to the types array

So now our manifest should look like this:

```json
{
    "id": "uinique.add-on.id",
    "version": "1.0.0",
    "name": "Hello, World",
    "description" : "My first Stremio add-on",
    "logo": "https://www.stremio.com/website/stremio-logo-small.png",
    "resources": ["catalog"],
    "types": ["movie"],
    "catalogs": [
        {"id": "movieCatalog", "type": "movie", "name": "Hello, Movies"}
    ]
}
```

The catalogs array describes our addon's catalogs. Every catalog must have `id` that is unique for the add-on, `type` that matches one of the types defined in the manifest and a human-readable `name`.

Now Stremio can search our add-on for a movie catalog.

## Populate the catalog

Stremio catalogs are structured like arrays of so called `meta preview`. It is preview because it contains only the minimal portion of information, required for our catalog to be rendered.

First inside the add-on directory we will create directory tree, starting with with our resource name - catalog. Inside we place a directory for each type we support. In our case this is only a movie directory. There we place our catalog file.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```bash
mkdir -p catalog/movie
```
<!--cmd-->
```batch
mkdir catalog\movie
```
<!--END_DOCUSAURUS_CODE_TABS-->

The name if the catalog's file must match the catalog `id`, that we defined in the manifest.

We will add two movies in our catalog. The contents of the file should look like this:

```json
{
    "metas": [
        { "type": "movie", "id": "tt0032138", "name": "The Wizard of Oz", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Adventure", "Family", "Fantasy", "Musical"] },
        { "type": "movie", "id": "tt0017136", "name": "Metropolis", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Drama", "Sci-Fi"] }
    ]
}
```

Write this into our `catalog/movie/movieCatalog.json`.

As you can see we have `type`, `id`, `name`, `poster` and `genres` for each item in the catalog.

The `type` should match the catalog type.

You can use any unique string for the `id`. In our case we use the corresponding IMDB ID. Stremio features an system add-on called `Cinemeta`. This add-on provides detailed metadata for any movie or TV show that matches valid IMDB ID.

Srtemio's catalog consists of grid of images, fetched from the `poster` field of every item. It should be valid URL to an image.

The `name` and `genres` are just human-readable descriptive fields. The `genres` are also used as filters in the `Discover` tab.

Summary
---

Now we have working catalog. If you reload the add-on (TODO: This is a hard task as of now), you will see our new catalog is displayed at the bottom of the `Board` and as a filter in the `Discover` tab.
