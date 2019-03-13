---
title: 3. The catalog
---

We covered the basic structure of the add-on manifest. Now it is time to make your add-on useful.

At this step, you will create a catalog. The catalog is a list of meta items grouped by their type. It can be a list of movies, TV shows and more.

Let's create a catalog.

## Deciding the types

The first thing to consider when creating an add-on catalog is what types of media will be provided.

The type determines the way the content is presented and organized within the app.

`movie` - Presents a single movie. This type doesn't provide any `video` objects. The sources are directly linked to the meta item.

`series` - Used for TV shows and the like. The videos are organized in seasons.

`channel` - Channel of videos. It's very similar to series but the videos are listed in a single list.

`tv` - Used for live TV. Similar to movie, but the sources are expected to be live-streamed without duration.

You can choose one or more types. For this basic example, we will use only the movie type.

## Update the manifest

As you are adding more features to your add-on, you must update the manifest so Stremio is be able to recognize and use these features.

We need to do several changes.

 * Add `catalog` to the resources array
 * Add `catalogs` array with description of our catalog
 * Add `movie` to the types array

So now our manifest should look like this:

```json
{
    "id": "my.first.stremio.add-on",
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

The `catalogs` array describes our addon's catalogs. Every catalog must have `id` that is unique for the add-on, `type` that matches one of the types defined in the manifest and a human-readable `name`.

Now Stremio can search our add-on for a movie catalog.

## Populate the catalog

Stremio catalogs are structured like arrays of the so called `meta preview`. It is a preview because it contains only the minimal amount of information required for the catalog to be rendered.

First inside the add-on directory you will create directory tree, starting with the resource name - catalog. Inside, you place a directory for each supported type. In this case, this is only a movie directory. Thes is where you place your catalog file.

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

The name of the catalog's file must match the catalog `id` that you defined in the manifest.

This is your desired file structure:

    my-stremio-addon
    +-- manifest.json
    +-- catalog
        +-- movie
            +-- movieCatalog.json

Now add two movies to your catalog. The contents of the file should look like this:

```json
{
    "metas": [
        { "type": "movie", "id": "tt0032138", "name": "The Wizard of Oz", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Adventure", "Family", "Fantasy", "Musical"] },
        { "type": "movie", "id": "tt0017136", "name": "Metropolis", "poster": "https://images.metahub.space/poster/medium/tt0017136/img", "genres": ["Drama", "Sci-Fi"] }
    ]
}
```

Write this into the `catalog/movie/movieCatalog.json`.

As you can see, you have `type`, `id`, `name`, `poster` and `genres` for each item in the catalog.

The `type` should match the catalog type.

You can use any unique string for the `id`. In this case we use the corresponding IMDB ID. Stremio features an system add-on called `Cinemeta`. This add-on provides detailed metadata for any movie or TV show that matches a valid IMDB ID.

Srtemio's catalog consists of grid of images, fetched from the `poster` field of every item. It should be a valid URL to an image.

The `name` and `genres` are just human-readable descriptive fields. The `genres` are also used as filters in the `Discover` tab.

See it in action
---

Now go back to step 2 and install our add-on again. If you keep the `http-server` running, you can skip ahead to the installation part.

When you navigate to the Stremio **Board** you will see that your catalog is listed at the very bottom. The catalogs in the Board are listed in the order of the installation of add-ons.

![Basic catalog](/stremio-addon-guide/img/catalog.png)

Summary
---

Adding features in Stremio's add-ons involves updating the manifest by describing the feature. Everything else is structured data.

In the next step we will dive into the meta description objects.
