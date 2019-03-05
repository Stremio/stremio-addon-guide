---
title: Adding catalogs
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
// Docs: https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/requests/defineCatalogHandler.md
addon.defineCatalogHandler(({type, id}) => {
	if (type === "movie" && id === "top") {
		return Promise.resolve({ metas: [
			{
				id: "tt1254207",
				type: "movie",
				name: "The Big Buck Bunny",
				poster: "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Big_buck_bunny_poster_big.jpg/220px-Big_buck_bunny_poster_big.jpg"
			}
		] })
	} else {
		return Promise.resolve({ metas: [] })
	}
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

In fact you don't need to change your catalog description in order to handle a pagination. If your catalog exceeds 100 items you must split it into pages. The next page will be requested, when the user scrolls down past the last item on the current page.

These extra properties are passed to the catalog handler via the `extra` object. 

When a new page is requested, the `extra` object will receive a `skip` property, that is usually a multiple of 100 and indicates how many items you should skip before you return the next at most 100 items.

```js
addon.defineCatalogHandler(({type, id, extra}) => {
	if (type === "movie" && id === "top") {
		const skip = extra.skip || 0;
		const topMetas = []; // Populate metas from somewhere
		return Promise.resolve({ metas: topMetas.slice(skip, skip + 100) })
	} else {
		return Promise.resolve({ metas: [] })
	}
})

```

### Searching

@TODO: Refactor the SDK so we don't provide spaghetti code


In order to enable searching capability in our add-on, we are going to again update it's manifest. We need to add some "extra" configuration into the catalog definition.

```js
    "catalogs": [
        {
            "type": "movie",
            "id": "top",
			"extra": [
				{ name: "search", isRequired: false },
			]
        }
    ],
```

This engages the search capabilities in our catalog. Whenever the user enters a query in the Stremio's search box, our add-on will be queried for results.

The isRequired parameter indicates that searching is optional. If set to true the catalog will not be available in the Board and Discover tabs.

Just like the case with pagination, when a search query is available the `search` property will be available in the `extra` object.
