## Adding catalogs

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

### Multiple catalogs

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


### Extra properties

@TODO
