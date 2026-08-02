# Understanding GeoJSON Structures

GeoJSON is a JSON-based format used to represent geographic locations, shapes, and related information.

A GeoJSON object commonly contains:

* A **geometry**, such as a point or line.
* **Properties** that describe the geographic object.
* A **Feature** wrapper that combines geometry and properties.
* A **FeatureCollection** that contains multiple features.

---

## Coordinate Order

GeoJSON coordinates normally use this order:

```text
[longitude, latitude]
```

For example:

```json
[-81.6944, 41.4993]
```

In this coordinate:

* `-81.6944` is the longitude.
* `41.4993` is the latitude.

An optional third number may represent altitude:

```json
[-81.6944, 41.4993, 850]
```

---

# 1. Point

A `Point` represents one geographic position.

Its `coordinates` property contains a single coordinate pair.

## Point Geometry

```json
{
  "type": "Point",
  "coordinates": [-81.6944, 41.4993]
}
```

This example represents one location at longitude `-81.6944` and latitude `41.4993`.

A geometry object by itself does not contain descriptive properties. To attach information to the point, place it inside a `Feature`.

## Point Feature

```json
{
  "type": "Feature",
  "properties": {
    "name": "Cleveland",
    "state": "Ohio"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
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

The coordinates are connected in the order they appear:

1. The line begins at `[-81.7000, 41.5000]`.
2. It continues to `[-81.6500, 41.5200]`.
3. It ends at `[-81.6000, 41.5500]`.

## LineString Feature

```json
{
  "type": "Feature",
  "properties": {
    "name": "Example Route",
    "routeType": "Training",
    "active": true
  },
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200],
      [-81.6000, 41.5500]
    ]
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
  "properties": {
    "name": "Example Route Network",
    "lineCount": 2
  },
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
  }
}
```

---

# 4. Single Feature

A `Feature` represents one geographic item.

A Feature normally contains:

* `type`
* `properties`
* `geometry`

## General Feature Structure

```json
{
  "type": "Feature",
  "properties": {},
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

The top-level `type` must be `"Feature"`.

The geometry has its own `type`, such as:

* `"Point"`
* `"LineString"`
* `"MultiLineString"`

## Single-Feature Example

```json
{
  "type": "Feature",
  "properties": {
    "identifier": "FIX001",
    "name": "Example Fix",
    "category": "Waypoint"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

This object describes one geographic feature:

* The feature is named `Example Fix`.
* Its category is `Waypoint`.
* Its geometry is a single point.

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
      "properties": {},
      "geometry": {
        "type": "Point",
        "coordinates": [-81.6944, 41.4993]
      }
    },
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200]
        ]
      }
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
      "properties": {
        "identifier": "FIX001",
        "name": "Starting Point",
        "category": "Waypoint"
      },
      "geometry": {
        "type": "Point",
        "coordinates": [-81.7000, 41.5000]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "identifier": "ROUTE001",
        "name": "Primary Route",
        "category": "Route"
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200],
          [-81.6000, 41.5500]
        ]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "identifier": "NETWORK001",
        "name": "Alternate Routes",
        "category": "Route Network"
      },
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
  "properties": {
    "identifier": "ROUTE001",
    "name": "Northern Route",
    "description": "A sample training route",
    "active": true,
    "minimumAltitude": 5000,
    "maximumAltitude": 10000,
    "routeClass": "Primary",
    "remarks": null
  },
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [-81.7000, 41.5000],
      [-81.6500, 41.5200],
      [-81.6000, 41.5500]
    ]
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

Property names and values are defined by the application using the GeoJSON data. GeoJSON does not require specific property names.

## Empty Properties

A feature may have an empty properties object:

```json
{
  "type": "Feature",
  "properties": {},
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

The `properties` value may also be `null`:

```json
{
  "type": "Feature",
  "properties": null,
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

Using an empty object is often more convenient when properties may be added later.

---

# 7. Feature ID

A Feature may contain an optional top-level `id`.

The `id` is separate from the `properties` object.

```json
{
  "type": "Feature",
  "id": "FIX001",
  "properties": {
    "name": "Example Fix"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

An identifier may instead be stored in `properties`:

```json
{
  "type": "Feature",
  "properties": {
    "identifier": "FIX001",
    "name": "Example Fix"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [-81.6944, 41.4993]
  }
}
```

The application consuming the GeoJSON determines which approach is more appropriate.

---

# 8. Single MultiLineString Versus Multiple LineString Features

These structures may look similar, but they represent different concepts.

## One Feature with a MultiLineString

Use this structure when all lines are parts of the same geographic feature and share the same properties.

```json
{
  "type": "Feature",
  "properties": {
    "name": "Route System",
    "owner": "Example Organization"
  },
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
      "properties": {
        "name": "Route A",
        "status": "Active"
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200]
        ]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "name": "Route B",
        "status": "Inactive"
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.8000, 41.4000],
          [-81.7500, 41.4200]
        ]
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
      "properties": {
        "name": "Departure Point",
        "featureType": "Waypoint",
        "active": true
      },
      "geometry": {
        "type": "Point",
        "coordinates": [-81.7000, 41.5000]
      }
    },
    {
      "type": "Feature",
      "id": "LINE001",
      "properties": {
        "name": "Primary Path",
        "featureType": "Route",
        "minimumAltitude": 5000
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-81.7000, 41.5000],
          [-81.6500, 41.5200],
          [-81.6000, 41.5500]
        ]
      }
    },
    {
      "type": "Feature",
      "id": "MULTILINE001",
      "properties": {
        "name": "Alternate Path System",
        "featureType": "Route Network",
        "lineCount": 2
      },
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
      }
    }
  ]
}
```

---

# Summary

| Concept             | GeoJSON Type        | Purpose                                   |
| ------------------- | ------------------- | ----------------------------------------- |
| Point               | `Point`             | Represents one geographic position        |
| Line                | `LineString`        | Represents one connected path             |
| Multiple lines      | `MultiLineString`   | Represents multiple lines as one geometry |
| Single feature      | `Feature`           | Combines one geometry with properties     |
| Multiple features   | `FeatureCollection` | Stores multiple independent Features      |
| Feature information | `properties`        | Stores descriptive, non-geographic data   |

The main structural difference is:

```text
Point:
One position

LineString:
One connected sequence of positions

MultiLineString:
Multiple sequences of positions within one geometry

Feature:
One geometry plus one properties object

FeatureCollection:
Multiple complete Feature objects
```
