# Floorplans (GeoJSON)

Floorplans that can be uploaded and displayed on the LightFi dashboard (or used with the API) should be specified using the [GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) open standard format.
This means that all features are positioned in geographic location on the globe and can be shown on a real map.

LightFi can digitalise a floorplan on request, please contact us to arrange this. If you would prefer to construct your own floorplans, use the following specification as a guide.

## GeoJSON Format

The GeoJSON format can support many features and geometries but the primary one used for floorplan specification is the `Polygon` geometry feature. Below is a basic example of a floorplan containing one desk polygon:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "id": "unique_integer_or_string",
        "type": "furniture.desk"
        "name": "Example Desk"
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [
              -0.103376033390241,
              51.50396850728883
            ],
            [
              -0.103387610110273,
              51.50396879686363
            ],
            [
              -0.103386599740581,
              51.503986332674764
            ],
            [
              -0.103375018922765,
              51.50398604299897
            ],
            [
              -0.103376033390241,
              51.50396850728883
            ]
          ]
        ]
      }
    }
  ]
}
```

TIP: A useful site for testing your basic GeoJSON layout is [https://geojson.io](https://geojson.io)

### Feature properties

#### `id`
All polygons that might be assigned to a particular sensor e.g. room or desk polygons, must have a unique `id` assigned to them.
The `id` should be assigned as shown in the example above, should be unique (on the floorplan) and can be either integer or string type.
Note: the `id` can also be assigned at the root level of the feature rather than in the properties, if needed, we prefer keeping the feature properties together in the `"properties"` section.

#### `name`
It is possible to give the polygon feature a name, this can be useful for organising and identifying features on the floorplan, allowing rooms etc. to be given their name.
This is not currently used by the LightFi front-end but could be used in future.

#### `type`
The feature type allows the polygons to be visualised and used for interaction on the dashboard. All features should be assigned a type so that they can be properly displayed. Feature types are listed below.

### Feature types

All features should specify a type in order to be properly displayed.
Ideally a complete floorplan will specify each of these types (unless they do not exist in the real space) in order to present the richest experience to the user.

| Feature type  | `id` guide | Description   |
|-------------- | -------------- | -------------- |
| `area.floor`    | 1000-1999 | Should be used for a polygon covering the whole outline of the floor |
| `area.room`     | 2000-2999 | Each room should have a separate polygon of this type |
| `area.toilet`   | 2000-2999 | Similar to `area.room` but used for WCs to allow icon on the map |
| `area.toilet.w` | 2000-2999 | Similar to `area.toilet` used for female toilets |
| `area.toilet.m` | 2000-2999 | Similar to `area.toilet` used for male toilets |
| `area.toilet.d` | 2000-2999 | Similar to `area.toilet` used for disabled toilets |
| `area.stairs`   | none | Allows stairs icons to be added to the floorplan display |
| `area.lift`     | none | Allows lift/elevator icons to be added to the floorplan display |
| `wall`          | none | Adding walls to the floorplan gives a nicer 3D visualisation and feel for the user |
| `window`        | none | Similar to walls, gives greater richness to the floorplan if used in combination |
| `furniture.desk`| 90001-99999 | Each desk should have a separate polygon of this type |

Note: A unique `id` should be included for each polygon where interaction may be required (e.g. rooms/desks, see above), no specific value for the `id` is required, some typical values used on floorplans are shown for guidance.



