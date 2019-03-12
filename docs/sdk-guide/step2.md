---
title: 2. Adding catalogs
---

Catalogs are lists of meta items, which appear in Board, Discover and Search.

If you want to provide catalogs, you should have the `"catalog"` resource in your `resources` field in the [manifest](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/manifest.md), and you should have at least one entry in the manifest's `catalogs`.

If you've selected "catalog" in `addon-bootstrap`, you will have this already:

```javascript
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
		"catalog"
	],
	"types": [
		"movie",
		"series"
	],
	"name": "he",
	"description": "f"
}
```

Note the first element of the `catalogs` array. The `type` definition determines how it will be classified in Discover/Board/Search, and the `id` exists to enable you to have multiple catalogs per add-on.

If your add-on only has one catalog, we recommend setting the `id` to `"top"` - it's not needed, but it's the de-facto standard for saying "this is the best/top content from my add-on".

Now, let's handle this catalog:

```javascript
// Populate the catalog from somewhere
function getMoviesCatalog(catalogName) {
    let catalog;

    switch(catalogName) {
        case "top":
            catalog = [
                {
                    id: "tt1254207",
                    type: "movie",
                    name: "The Big Buck Bunny",
                    poster: "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Big_buck_bunny_poster_big.jpg/220px-Big_buck_bunny_poster_big.jpg",
                    genres: [ "Animation", "Short", "Comedy" ]
                },
            ]
            break
        default:
            catalog = []
            break
    }

    return Promise.resolve(catalog)
}

// Docs: https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/requests/defineCatalogHandler.md
builder.defineCatalogHandler(({type, id}) => {
    let results;

    switch(type) {
        case "movie":
            results = getMoviesCatalog(id)
            break
       default:
            results = Promise.resolve( [] )
            break
    }

    return results.then(items => ({
        metas: items
    }))
})
```

## Multiple catalogs

If you want to define multiple catalogs, you can do this by just adding another entry to your `manifest.catalogs`.

If your add-on provides multiple types of content (e.g. movies and series), then you have to provide your catalogs separately for each type, for example:

```
	// in the manifest definition
	"catalogs": [
		{ "type": "movie", "id": "top-by-popularity" },
		{ "type": "movie", "id": "top-by-rating" },
		{ "type": "series", "id": "top-by-popularity" },
		{ "type": "series", "id": "top-by-rating" },
	],
```

Now, all you have to do, is serve different contents depending on the `type` and `id` arguments of the handler you defined with `defineCatalogHandler`!


## Extra properties

In order to provide pagination, filtering and search, you can specify a set of [extra properties](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/manifest.md#extra-properties), for each catalog.

### Pagination

For the pagination to work correctly, you need to add the `skip` extra option to your catalog description inside the manifest.

```js
"catalogs": [
    {
        "type": "movie",
        "id": "top",
        "extra": [
            { "name": "skip", "isRequired": false },
        ]
    },
],
```

If your catalog exceeds 100 items you must split them into pages. The next page will be requested, when the user scrolls down past the last item on the current page.

These extra properties are passed to the catalog handler via the `extra` object.

When a new page is requested, the `extra` object will receive a `skip` property, that is usually a multiple of 100 and indicates how many items you should skip before you return the next at most 100 items.

```js
builder.defineCatalogHandler(({type, id, extra}) => {
    let results;

    switch(type) {
        case "movie":
            results = getMoviesCatalog(id)
            break
       default:
            results = Promise.resolve( [] )
            break
    }

    const skip = extra.skip || 0;
    return results.then(items => ({
        metas: items.slice(skip, skip + 100)
    }))
})
```

### Searching

In order to enable searching capability in our add-on, we are going to again update it's manifest. We need to add some "extra" configuration into the catalog definition.

```js
"catalogs": [
    {
        "type": "movie",
        "id": "top",
        "extra": [
            { "name": "skip", "isRequired": false },
            { "name": "search", "isRequired": false },
        ]
    },
],
```

This engages the search capabilities in our catalog. Whenever the user enters a query in the Stremio's search box, our add-on will be queried for results.

The isRequired parameter indicates that searching is optional. If set to true the catalog will not be available in the Board and Discover tabs.

Just like the case with pagination, when a search query is available the `search` property will be present in the `extra` object.

```js
builder.defineCatalogHandler(({type, id, extra}) => {
    let results;

    switch(type) {
        case "movie":
            results = getMoviesCatalog(id)
            break
       default:
            results = Promise.resolve( [] )
            break
    }
    if(extra.search) {
        return results.then(items => ({
            metas: items.filter(meta => meta.name
            .toLowercase()
            .includes(extra.search.toLowercase()))
        }))
    }

    const skip = extra.skip || 0;
    return results.then(items => ({
        metas: items.slice(skip, skip + 100)
    }))
 })
```

### Genre filters

The genre filters are very similar to searching. We just need to add one more thing - a list of genres, our add-on is aware of.

```js
"catalogs": [
    {
        "type": "movie",
        "id": "top",
        "extra": [
            { "name": "search", "isRequired": false },
            { "name": "genre", "isRequired": false },
        ],
        "genres": [ "Animation", "Comedy" ]
    },
],
```

We don't list all genres of the content we provide. We list only the ones our catalog is capable of filtering.

> Note
>
> `isRequired` must not be set to `true` on more than one extra paremeter. Otherwise your catalog will be ignored.

And this is the final result:

```js
builder.defineCatalogHandler(({type, id, extra}) => {
    let results;

    switch(type) {
        case "movie":
            results = getMoviesCatalog(id)
            break
       default:
            results = Promise.resolve( [] )
            break
    }
    if(extra.search) {
        return results.then(items => {
            metas: items.filter(meta => meta.name
            .toLowercase()
            .includes(extra.search.toLowercase()))
        })
    } else if(extra.genre) {
        return results.then(items => ({
            metas: items.filter(meta => meta.genres
            .includes(extra.genre))
        }))
    }

    const skip = extra.skip || 0;
    return results.then(items => ({
        metas: items.slice(skip, skip + 100)
    }))
 })
```

