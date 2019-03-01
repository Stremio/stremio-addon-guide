---
id: step7
title: Step 7 - Dynamic content
---

TODO: this chapter

There are cases where you will want to do some server side work so your add-on can provide better user experience. For example there is a way to paginate the catalogs. You can also provide search and filtering by genre, to name a few.

In fact here will take a look exactly at these scenarios.

Searching
---

In order to enable searching capability in our add-on, we are going to yet again update it's manifest. We need to add some "extra" configuration into the catalog definition.

```js
"catalogs": [
    {
        "id": "movieCatalog", "type": "movie", "name": "Hello, Movies", 
        "extra": [
            { "name": "search", "isRequired": false }
        ]
    },
    {"id": "seriesCatalog", "type": "series", "name": "Hello, TV Shows"}
]
```

This engages the search capabilities in our catalog. Whenever the user enters a query in the Stremio's search box, our add-on will be queried for results.

The `isRequired` parameter indicates that searching is optional. If set to `true` the catalog will not be available in the Board and Discover tabs.

### Update the manifest

Now our `manifest.json` should look like this:

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
        {
            "id": "movieCatalog", "type": "movie", "name": "Hello, Movies",
            "extra": [
                { "name": "search", "isRequired": false }
            ]

        },
        {"id": "seriesCatalog", "type": "series", "name": "Hello, TV Shows"}
    ]
}
```

### Try searching

Let's see what will happen. Reinstall our add-on now. Now try searching for "jellyfish", for example. In the web server logs you will see the following line:

```
"GET /catalog/movie/movieCatalog/search=jellyfish.json" Error (404): "Not found"
```

Here we can see how does Stremio queries our catalog for search results. The series are not pulled because we added this capability only to our movie catalog.

### Provide search results

Let's copy our movie catalog in a file that would match a search query.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```sh
mkdir catalog/movie/movieCatalog
cp catalog/movie/movieCatalog.json catalog/movie/movieCatalog/search=hello.json
```
<!--cmd-->
```batch
mkdir catalog\movie\movieCatalog
copy catalog\movie\movieCatalog.json catalog\movie\movieCatalog\search=hello.json
```
<!--END_DOCUSAURUS_CODE_TABS-->

You can try searching for "hello" (There is no need to reinstall the add-on as long as you do not change the manifest). In the web server's log there is no error now. You can see that the last row of search results consists of our movie catalog.

![Search results](img/search.png)

Genre filters
---

The genre filters are very similar to searching. We just need to add one more thing - a list of genres, our add-on is aware of.

```json
{
    "id": "movieCatalog", "type": "movie", "name": "Hello, Movies", 
    "extra": [
        { "name": "search", "isRequired": false },
        { "name": "genre", "isRequired": false }
    ],
    "genres" : ["Adventure", "Family", "Sci-Fi", "Demo"]
}
```

We don't list all genres of the content we provide. We list only the ones we can handle.

> Note
>
> `isRequired` must not be set to `true` on more than one extra paremeter. Otherwise your catalog will be ignored.

### Update the manifest

Yet again our manifest requires changes. This is it's new form:

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
        {
            "id": "movieCatalog", "type": "movie", "name": "Hello, Movies",
            "extra": [
                { "name": "search", "isRequired": false },
                { "name": "genre", "isRequired": false }
            ],
            "genres" : ["Adventure", "Family", "Sci-Fi", "Demo"]
        },
        {"id": "seriesCatalog", "type": "series", "name": "Hello, TV Shows"}
    ]
}
```

### Filter by genre

We will now create smaller catalogs with only the matching meta items.

Let's create a file called `catalog/movie/movieCatalog/genre=Adventure.json` and put there an one-item catalog with only "The Wizard of Oz":

```json
{
    "metas": [
        { "type": "movie", "id": "tt0032138", "name": "The Wizard of Oz", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Adventure", "Family", "Fantasy", "Musical"] }
    ]
}
```

The next is `catalog/movie/movieCatalog/genre=Sci-Fi.json` containing "Metropolis":

```json
{
    "metas": [
        { "type": "movie", "id": "tt0017136", "name": "Metropolis", "poster": "https://images.metahub.space/poster/medium/tt0017136/img", "genres": ["Drama", "Sci-Fi"] }
    ]
}
```

We will add "The Wizard of Oz" and our "Jellyfish" into the `catalog/movie/movieCatalog/genre=Family.json`. The "Jellyfish" video doesn't match that genre, but it will be shown if it is in the catalog:

```json
{
    "metas": [
        { "type": "movie", "id": "tt0032138", "name": "The Wizard of Oz", "poster": "https://images.metahub.space/poster/medium/tt0032138/img", "genres": ["Adventure", "Family", "Fantasy", "Musical"] },
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

### Check out the result

As we did changes to the manifest, the add-on have to be reinstalled again.

If you then open our movie catalog, inside the Discover tab in Stremio, you will see the list of our genres.

### Handle empty catalogs
We have stated that this catalog provides a filter for one more genre - the "Demo" genre, but we haven't made a filtered catalog for this genre. It you try to use that filter you will get an error:

![Error loading catalog](img/genre_error.png)

> **Note**
>
> When you define genre filter, your add-on must provide a valid response. Otherwise an error message will appear.

Let's suppose that we have defined that genre as supported, but we don't have items to display yet? In order to avid this error, you must return a empty catalog like this:

```json
{
    "metas": []
}
```

Put this in `catalog/movie/movieCatalog/genre=Demo.json` so you'll handle the empty catalog case.

Pagination
---

When you have a large amount of items in your catalog, it's better to split it in several smaller catalogs. Stremio will fetch them sequentially when the user scrolls past the last result.

Every catalog must be at maximum of 100 items. When the user scrolls past the current catalog a request will be made for more data.

For example, let's say that our catalog has 75 items in it. When the user scrolls past these 75 items, Stremio wiil ask for the next batch:

```
GET /catalog/movie/movieCatalog.json
```

And then

```
GET /catalog/movie/movieCatalog/skip=75.json
```

If here we return 40 more items, the next request will be:

```
GET /catalog/movie/movieCatalog/skip=115.json
```

This will continue until we return an empty catalog:

```json
{
    "metas": []
}
```

Summary
---

A Stremio add-on can be crafted to be entirely static. Of course this is not the advised way. For example if you keep on high quality user experience, you will probably want to provide at least a fuzzy word search, so the typos will not affect the result so much.

From now on it's up to you how your add-on will perform against the competition.
