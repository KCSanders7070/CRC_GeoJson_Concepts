# Understanding GeoJSON Structures

GeoJSON is a JSON-based format used to represent geographic locations, shapes, and related information.

A GeoJSON file commonly contains a feature collection with one or more features:

- **FEATURECOLLECTION** `contains multiple features`
  - **FEATURE** `combines geometry and properties`
    - **GEOMETRY** `point, line, or polygon`
    - **PROPERTIES** `describes the geographic object`
- A Feature must contain a `type`, `geometry`, and a `properties`.
  - `geometry` may be null, while `properties` must be either a JSON object or null.
  - Order does not matter but is commonly formated as `type`, `geometry`, `properties`.

---

## Coordinate Order

GeoJSON coordinates normally use `[longitude, latitude]`  
For example: `[-81.6944, 41.4993]`

An optional third number may represent altitude meters above or below the WGS 84 reference ellipsoid: `[-81.6944, 41.4993, 850]`
- Note: Alittude aspect not recognized in CRC, yet.

---

# 1. Point

A `Point` represents a single geographic position.  
Its `coordinates` property contains a single coordinate pair.

## Point Geometry

```json
{
  "type": "Point",
  "coordinates": [-81.6944, 41.4993]
}
```

A geometry object by itself does not contain descriptive properties. To attach information to the point, place it inside a `Feature`.

## Point Feature

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": {
    "name": "Cleveland",
    "state": "Ohio"
  }
}
```

---

# 2. LineString

A `LineString` represents a path made from two or more connected positions.

Its `coordinates` property contains an array of coordinate pairs.

## LineString Geometry

```json
{
  "type": "LineString",
  "coordinates": [
    [-81.7000, 41.5000],
    [-81.6500, 41.5200],
    [-81.6000, 41.5500]
  ]
}
```

## LineString Feature

```json
{
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200],
      [-81.6000, 41.5500]
    ]
  },
  "properties": {
    "name": "Example Route",
    "routeType": "Training",
    "active": true
  }
}
```

---

# 3. MultiLineString

A `MultiLineString` represents multiple separate lines within one geometry.

Its `coordinates` property contains:

1. An array of lines.
2. Each line contains an array of coordinate pairs.
3. Each coordinate pair contains longitude and latitude.


## MultiLineString Geometry

```json
{
  "type": "MultiLineString",
  "coordinates": [
    [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200],
      [-81.6000, 41.5500]
    ],
    [
      [-81.8000, 41.4000],
      [-81.7500, 41.4200],
      [-81.7000, 41.4500]
    ]
  ]
}
```

This geometry contains two separate lines.

The first line is:

```json
[
  [-81.7000, 41.5000],
  [-81.6500, 41.5200],
  [-81.6000, 41.5500]
]
```

The second line is:

```json
[
  [-81.8000, 41.4000],
  [-81.7500, 41.4200],
  [-81.7000, 41.4500]
]
```

The end of the first line is not automatically connected to the beginning of the second line.

## MultiLineString Feature

```json
{
  "type": "Feature",
  "geometry": {
    "type": "MultiLineString",
    "coordinates": [
      [
        [-81.7000, 41.5000],
        [-81.6500, 41.5200],
        [-81.6000, 41.5500]
      ],
      [
        [-81.8000, 41.4000],
        [-81.7500, 41.4200],
        [-81.7000, 41.4500]
      ]
    ]
  },
  "properties": {
    "name": "Example Route Network",
    "lineCount": 2
  }
}
```
- Note: In CRC, a MultiLineString feature is a great way to store things such as an airway that has one or more "breaks" or to construct a SID/STAR with different paths that aren't all one single continuous line.

---

# 4. Single Feature

A `Feature` represents one geographic item.

A Feature normally contains:

* `type`
* `geometry`
* `properties`

## General Feature Structure

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": {}
}
```

The top-level `type` must be `"Feature"`.

The geometry has its own `type`, such as:

* `"Point"`
* `"LineString"`
* `"MultiLineString"`
* `"Polygon"`

## Single-Feature Example

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": {
    "identifier": "FIX001",
    "name": "Example Fix",
    "category": "Waypoint"
  }
}
```

---

# 5. Multiple Features

Multiple independent features are stored in a `FeatureCollection`.

A `FeatureCollection` contains a `features` array. Each item in that array is a complete GeoJSON Feature.

## General FeatureCollection Structure

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-81.6944, 41.4993]
      },
      "properties": {}
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200]
        ]
      },
      "properties": {}
    }
  ]
}
```

## Multiple-Feature Example

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-81.7000, 41.5000]
      },
      "properties": {
        "identifier": "FIX001",
        "name": "Starting Point",
        "category": "Waypoint"
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200],
          [-81.6000, 41.5500]
        ]
      },
      "properties": {
        "identifier": "ROUTE001",
        "name": "Primary Route",
        "category": "Route"
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "MultiLineString",
        "coordinates": [
          [
            [-81.8000, 41.4000],
            [-81.7500, 41.4200]
          ],
          [
            [-81.6000, 41.5500],
            [-81.5500, 41.5800]
          ]
        ]
      },
      "properties": {
        "identifier": "NETWORK001",
        "name": "Alternate Routes",
        "category": "Route Network"
      }
    }
  ]
}
```

This FeatureCollection contains three independent features:

1. A Point named `Starting Point`.
2. A LineString named `Primary Route`.
3. A MultiLineString named `Alternate Routes`.

Each feature has its own properties and geometry.

---

# 6. Feature Properties

The `properties` object stores descriptive information about a feature.

Properties do not define where the feature is located. The `geometry` object handles the geographic information.

## Properties Example

```json
{
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200],
      [-81.6000, 41.5500]
    ]
  },
  "properties": {
    "identifier": "ROUTE001",
    "name": "Northern Route",
    "description": "A sample training route",
    "active": true,
    "minimumAltitude": 5000,
    "maximumAltitude": 10000,
    "routeClass": "Primary",
    "remarks": null
  }
}
```

Properties can contain normal JSON value types:

```json
{
  "textValue": "Example",
  "numberValue": 5000,
  "decimalValue": 123.45,
  "booleanValue": true,
  "nullValue": null,
  "arrayValue": ["A", "B", "C"],
  "objectValue": {
    "source": "Training Data",
    "revision": 1
  }
}
```

Property names and values are defined by the application using the GeoJSON data, such as CRC. GeoJSON does not require specific property names.

## Empty Properties

A feature may have an empty properties object:

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": {}
}
```

The `properties` value may also be `null`:

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": null
}
```

Note: Using an empty object, rather than null, is often more convenient when properties may be added later.

---

# 7. Feature ID

If your feature has an identifier of some sort and you do not wish to list the ID as a property, a Feature may contain an optional top-level `id` and may be either a JSON string or number.

The specific `id` key is separate from the `properties` object.

```json
{
  "type": "Feature",
  "id": "FIX001",
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  },
  "properties": {
    "name": "Example Fix"
  }
}
```

The application consuming the GeoJSON determines which approach is more appropriate.  Note: CRC does not make use of the `id` key.

---

# 8. Single MultiLineString Versus Multiple LineString Features

These structures may look similar, but they represent different concepts.

## One Feature with a MultiLineString

Use this structure when all lines are parts of the same geographic feature and share the same properties.

```json
{
  "type": "Feature",
  "geometry": {
    "type": "MultiLineString",
    "coordinates": [
      [
        [-81.7000, 41.5000],
        [-81.6500, 41.5200]
      ],
      [
        [-81.8000, 41.4000],
        [-81.7500, 41.4200]
      ]
    ]
  },
  "properties": {
    "name": "Route System",
    "owner": "Example Organization"
  }
}
```

Both lines share these properties:

```json
{
  "name": "Route System",
  "owner": "Example Organization"
}
```

## Multiple Features with LineStrings

Use a FeatureCollection when each line is a separate feature with its own properties.

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200]
        ]
      },
      "properties": {
        "name": "Route A",
        "status": "Active"
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.8000, 41.4000],
          [-81.7500, 41.4200]
        ]
      },
      "properties": {
        "name": "Route B",
        "status": "Inactive"
      }
    }
  ]
}
```

In this version:

* `Route A` has its own properties.
* `Route B` has its own properties.
* The two routes can be processed independently.

---

# 9. Geometry Nesting Comparison

The number of nested coordinate arrays depends on the geometry type.

## Point

One coordinate pair:

```json
{
  "type": "Point",
  "coordinates": [-81.7000, 41.5000]
}
```

Coordinate structure:

```json
[longitude, latitude]
```

## LineString

An array of coordinate pairs:

```json
{
  "type": "LineString",
  "coordinates": [
    [-81.7000, 41.5000],
    [-81.6500, 41.5200]
  ]
}
```

Coordinate structure:

```json
[
  [longitude, latitude],
  [longitude, latitude]
]
```

## MultiLineString

An array of LineString coordinate arrays:

```json
{
  "type": "MultiLineString",
  "coordinates": [
    [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200]
    ],
    [
      [-81.8000, 41.4000],
      [-81.7500, 41.4200]
    ]
  ]
}
```

Coordinate structure:

```json
[
  [
    [longitude, latitude],
    [longitude, latitude]
  ],
  [
    [longitude, latitude],
    [longitude, latitude]
  ]
]
```

---

# 10. Complete Combined Example

The following example combines Points, LineStrings, MultiLineStrings, and feature properties in one FeatureCollection.

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": "POINT001",
      "geometry": {
        "type": "Point",
        "coordinates": [-81.7000, 41.5000]
      },
      "properties": {
        "name": "Departure Point",
        "featureType": "Waypoint",
        "active": true
      }
    },
    {
      "type": "Feature",
      "id": "LINE001",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200],
          [-81.6000, 41.5500]
        ]
      },
      "properties": {
        "name": "Primary Path",
        "featureType": "Route",
        "minimumAltitude": 5000
      }
    },
    {
      "type": "Feature",
      "id": "MULTILINE001",
      "geometry": {
        "type": "MultiLineString",
        "coordinates": [
          [
            [-81.8000, 41.4000],
            [-81.7500, 41.4200],
            [-81.7000, 41.4500]
          ],
          [
            [-81.6000, 41.5500],
            [-81.5500, 41.5800],
            [-81.5000, 41.6000]
          ]
        ]
      },
      "properties": {
        "name": "Alternate Path System",
        "featureType": "Route Network",
        "lineCount": 2
      }
    }
  ]
}
```

---

# Summary

| Concept             | GeoJSON Type        | Purpose                                                                                  |
| ------------------- | ------------------- | ---------------------------------------------------------------------------------------- |
| Point               | `Point`             | Represents one geographic position                                                       |
| Line                | `LineString`        | Represents one connected path                                                            |
| Multiple lines      | `MultiLineString`   | Represents multiple lines as one geometry                                                |
| Polygon             | `Polygon`           | Represents an enclosed area using one exterior boundary and optional interior boundaries |
| Single feature      | `Feature`           | Combines one geometry with properties                                                    |
| Multiple features   | `FeatureCollection` | Stores multiple independent Features                                                     |
| Feature information | `properties`        | Stores descriptive, non-geographic data                                                  |

The main structural difference is:

```text
Point:
One position

LineString:
One connected sequence of positions

MultiLineString:
Multiple sequences of positions within one geometry

Polygon:
One exterior boundary with optional interior boundaries

Feature:
One geometry plus one properties object

FeatureCollection:
Multiple complete Feature objects
```
