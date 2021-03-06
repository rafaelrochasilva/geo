# Geo

[![Build Status](https://travis-ci.org/bryanjos/geo.svg?branch=master)](https://travis-ci.org/bryanjos/geo)

A collection of GIS functions. Handles conversions to and from WKT, WKB, and GeoJSON for the following geometries:

* Point
* LineString
* Polygon
* MultiPoint
* MulitLineString
* MultiPolygon
* GeometryCollection

*Note*: If you are looking for the Postgrex PostGIS extension, check out [geo_postgis](https://github.com/bryanjos/geo_postgis)

*Note*: If you are looking to do geospatial calculations in memory with Geo's structs, check out [topo](https://github.com/pkinney/topo)

```elixir
defp deps do
  [{:geo, "~> 2.0"}]
end
```

[Documentation](http://hexdocs.pm/geo)


## Examples


* Encode and decode WKT and EWKT

  ```elixir
  iex(1)> point = Geo.WKT.decode("POINT(30 -90)")
  %Geo.Point{ coordinates: {30, -90}, srid: nil}

  iex(2)> Geo.WKT.encode(point)
  "POINT(30 -90)"

  iex(3)> point = Geo.WKT.decode("SRID=4326;POINT(30 -90)")
  %Geo.Point{coordinates: {30, -90}, srid: 4326}
  ```


* Encode and decode WKB and EWKB

  ```elixir
  iex(1)> point = Geo.WKB.decode("0101000000000000000000F03F000000000000F03F")
  %Geo.Point{ coordinates: {1.0, 1.0}, srid: nil }

  iex(2)> Geo.WKB.encode(point)
  "00000000013FF00000000000003FF0000000000000"

  iex(3)> point = Geo.WKB.decode("0101000020E61000009EFB613A637B4240CF2C0950D3735EC0")
  %Geo.Point{ coordinates: {36.9639657, -121.8097725}, srid: 4326 }

  iex(4)> Geo.WKB.encode(point)
  "0020000001000010E640427B633A61FB9EC05E73D350092CCF"
  ```

* Encode and decode GeoJSON


  Geo only encodes and decodes maps shaped as GeoJSON. JSON encoding and decoding must
  be done before and after.

  ```elixir
  #Examples using Poison as the JSON parser

  iex(1)> Geo.JSON.encode(point)
  %{ "type" => "Point", "coordinates" => [100.0, 0.0] }

  iex(2)> point = Poison.decode!("{ \"type\": \"Point\", \"coordinates\": [100.0, 0.0] }") |> Geo.JSON.decode
  %Geo.Point{ coordinates: {100.0, 0.0}, srid: nil }

  iex(3)> Geo.JSON.encode(point) |> Poison.encode!
  "{\"type\":\"Point\",\"coordinates\":[100.0,0.0]}"
  ```

## Development

After you got the dependencies via `mix deps.get` make sure that:

* `postgis` is installed
* your `postgres` user has the database `"geo_postgrex_test"`
* your `postgres` db user can login without a password or you set the `PGPASSWORD` environment variable appropriately

Then you can run the tests as you are used to with `mix test`.
