
# Chicago Crime Record (Schema)

`iguide.bbr.chicagoCrime` *v0.1*

Schema and semantic annotation for a single reported crime incident from the City of Chicago 'Crimes - 2001 to Present' dataset, as published on the I-GUIDE Platform (Chicago Crime data 2026).

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Chicago Crime Record

Describes a single reported crime incident from the City of Chicago
[Crimes - 2001 to Present](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2)
dataset. The I-GUIDE Platform publishes yearly extracts of it; this building block was created for
[Chicago Crime data 2026](https://platform.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627)
(source: Chicago Data Portal, contributed by Yunfan Kang), but it applies to any year.

### Data access

```python
from urllib.request import urlretrieve
urlretrieve("https://storage.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627/export.csv")
```

### Mapping from the CSV export

The CSV export stores every value as a quoted string. This building block defines a typed JSON
representation whose property names match the Chicago Data Portal API field names:

| CSV column | Property | Type |
|---|---|---|
| ID | `id` | string (digits) |
| Case Number | `case_number` | string |
| Date | `date` | ISO 8601 local date-time |
| Block | `block` | string |
| IUCR | `iucr` | string (4 characters) |
| Primary Type | `primary_type` | string |
| Description | `description` | string |
| Location Description | `location_description` | string or null |
| Arrest | `arrest` | boolean |
| Domestic | `domestic` | boolean |
| Beat | `beat` | string (4 digits, leading zeros kept) |
| District | `district` | string (3 digits) or null |
| Ward | `ward` | integer 1–50 or null |
| Community Area | `community_area` | integer 1–77 or null |
| FBI Code | `fbi_code` | string (e.g. `08B`) |
| X Coordinate / Y Coordinate | `x_coordinate` / `y_coordinate` | number (EPSG:3435, US feet) or null |
| Year | `year` | integer |
| Updated On | `updated_on` | ISO 8601 local date-time |
| Latitude / Longitude | `latitude` / `longitude` | number (WGS 84) or null |
| Location (WKT) | `location` | GeoJSON Point or null |

Date-times are Chicago local time and carry no UTC offset. Locations are shifted from the actual
address for privacy, but stay on the same block.

### Semantics and validation

The JSON-LD context maps identifiers and timestamps to Dublin Core terms, latitude/longitude to the
W3C WGS 84 vocabulary, the location to GeoSPARQL `geo:hasGeometry`, and the remaining fields to a
dataset-specific namespace (`https://platform.i-guide.io/def/chicago-crime/`). SHACL rules check that
each record has a single identifier and case number, and that coordinates fall within Chicago's
bounding box.

## Examples

### Geolocated crime record
A criminal damage incident from the 2026 extract, converted from the CSV export
to typed JSON with a GeoJSON Point location.
#### json
```json
{
  "id": "14276240",
  "case_number": "JK350788",
  "date": "2026-07-26T00:00:00",
  "block": "012XX W MARQUETTE RD",
  "iucr": "1310",
  "primary_type": "CRIMINAL DAMAGE",
  "description": "TO PROPERTY",
  "location_description": "APARTMENT",
  "arrest": false,
  "domestic": false,
  "beat": "0724",
  "district": "007",
  "ward": 17,
  "community_area": 67,
  "fbi_code": "14",
  "x_coordinate": 1169332,
  "y_coordinate": 1860356,
  "year": 2026,
  "updated_on": "2026-08-02T15:44:42",
  "latitude": 41.772324861,
  "longitude": -87.654831089,
  "location": {
    "type": "Point",
    "coordinates": [-87.654831089, 41.772324861]
  }
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "ccr": "https://platform.i-guide.io/def/chicago-crime/",
      "dct": "http://purl.org/dc/terms/",
      "geo": "http://www.opengis.net/ont/geosparql#",
      "wgs84": "http://www.w3.org/2003/01/geo/wgs84_pos#"
    },
    "https://raw.githubusercontent.com/I-GUIDE/ogc-bblock-iguide/undefined/build/annotated/bbr/chicagoCrime/context.jsonld"
  ],
  "id": "14276240",
  "case_number": "JK350788",
  "date": "2026-07-26T00:00:00",
  "block": "012XX W MARQUETTE RD",
  "iucr": "1310",
  "primary_type": "CRIMINAL DAMAGE",
  "description": "TO PROPERTY",
  "location_description": "APARTMENT",
  "arrest": false,
  "domestic": false,
  "beat": "0724",
  "district": "007",
  "ward": 17,
  "community_area": 67,
  "fbi_code": "14",
  "x_coordinate": 1169332,
  "y_coordinate": 1860356,
  "year": 2026,
  "updated_on": "2026-08-02T15:44:42",
  "latitude": 41.772324861,
  "longitude": -87.654831089,
  "location": {
    "type": "Point",
    "coordinates": [
      -87.654831089,
      41.772324861
    ]
  }
}
```

#### ttl
```ttl
@prefix ccr: <https://platform.i-guide.io/def/chicago-crime/> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix geo: <http://www.opengis.net/ont/geosparql#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix wgs84: <http://www.w3.org/2003/01/geo/wgs84_pos#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

[] dcterms:identifier "14276240" ;
    dcterms:modified "2026-08-02T15:44:42"^^xsd:dateTime ;
    geo:hasGeometry "{\"coordinates\":[-87.654831089,41.772324861],\"type\":\"Point\"}"^^rdf:JSON ;
    wgs84:lat 41.772324861 ;
    wgs84:long -87.654831089 ;
    ccr:arrest false ;
    ccr:beat "0724" ;
    ccr:block "012XX W MARQUETTE RD" ;
    ccr:caseNumber "JK350788" ;
    ccr:communityArea 67 ;
    ccr:date "2026-07-26T00:00:00"^^xsd:dateTime ;
    ccr:description "TO PROPERTY" ;
    ccr:district "007" ;
    ccr:domestic false ;
    ccr:fbiCode "14" ;
    ccr:iucr "1310" ;
    ccr:locationDescription "APARTMENT" ;
    ccr:primaryType "CRIMINAL DAMAGE" ;
    ccr:ward 17 ;
    ccr:xCoordinate 1169332.0 ;
    ccr:yCoordinate 1860356.0 ;
    ccr:year "2026"^^xsd:gYear .


```


### Crime record without location
Some records (e.g. certain domestic incidents) are published without coordinates.
Location-related properties are then `null`.
#### json
```json
{
  "id": "14281178",
  "case_number": "JK355713",
  "date": "2026-07-26T00:00:00",
  "block": "054XX N EAST RIVER RD",
  "iucr": "0486",
  "primary_type": "BATTERY",
  "description": "DOMESTIC BATTERY SIMPLE",
  "location_description": "APARTMENT",
  "arrest": false,
  "domestic": true,
  "beat": "1614",
  "district": "016",
  "ward": 41,
  "community_area": 76,
  "fbi_code": "08B",
  "x_coordinate": null,
  "y_coordinate": null,
  "year": 2026,
  "updated_on": "2026-08-02T15:44:42",
  "latitude": null,
  "longitude": null,
  "location": null
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "ccr": "https://platform.i-guide.io/def/chicago-crime/",
      "dct": "http://purl.org/dc/terms/",
      "geo": "http://www.opengis.net/ont/geosparql#",
      "wgs84": "http://www.w3.org/2003/01/geo/wgs84_pos#"
    },
    "https://raw.githubusercontent.com/I-GUIDE/ogc-bblock-iguide/undefined/build/annotated/bbr/chicagoCrime/context.jsonld"
  ],
  "id": "14281178",
  "case_number": "JK355713",
  "date": "2026-07-26T00:00:00",
  "block": "054XX N EAST RIVER RD",
  "iucr": "0486",
  "primary_type": "BATTERY",
  "description": "DOMESTIC BATTERY SIMPLE",
  "location_description": "APARTMENT",
  "arrest": false,
  "domestic": true,
  "beat": "1614",
  "district": "016",
  "ward": 41,
  "community_area": 76,
  "fbi_code": "08B",
  "x_coordinate": null,
  "y_coordinate": null,
  "year": 2026,
  "updated_on": "2026-08-02T15:44:42",
  "latitude": null,
  "longitude": null,
  "location": null
}
```

#### ttl
```ttl
@prefix ccr: <https://platform.i-guide.io/def/chicago-crime/> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix geo: <http://www.opengis.net/ont/geosparql#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

[] dcterms:identifier "14281178" ;
    dcterms:modified "2026-08-02T15:44:42"^^xsd:dateTime ;
    geo:hasGeometry "null"^^rdf:JSON ;
    ccr:arrest false ;
    ccr:beat "1614" ;
    ccr:block "054XX N EAST RIVER RD" ;
    ccr:caseNumber "JK355713" ;
    ccr:communityArea 76 ;
    ccr:date "2026-07-26T00:00:00"^^xsd:dateTime ;
    ccr:description "DOMESTIC BATTERY SIMPLE" ;
    ccr:district "016" ;
    ccr:domestic true ;
    ccr:fbiCode "08B" ;
    ccr:iucr "0486" ;
    ccr:locationDescription "APARTMENT" ;
    ccr:primaryType "BATTERY" ;
    ccr:ward 41 ;
    ccr:year "2026"^^xsd:gYear .


```


### Converting the I-GUIDE CSV export
The dataset is distributed on the I-GUIDE Platform as a CSV file in which every value is a
string. This script downloads it and converts each row into a record conforming to this
building block.
#### python
```python
import csv, json
from datetime import datetime
from urllib.request import urlretrieve

URL = "https://storage.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627/export.csv"
path, _ = urlretrieve(URL)

def dt(s):
    return datetime.strptime(s, "%m/%d/%Y %I:%M:%S %p").isoformat() if s else None

def num(s, cast=float):
    return cast(s) if s else None

def to_record(r):
    lat, lon = num(r["Latitude"]), num(r["Longitude"])
    return {
        "id": r["ID"],
        "case_number": r["Case Number"],
        "date": dt(r["Date"]),
        "block": r["Block"],
        "iucr": r["IUCR"],
        "primary_type": r["Primary Type"],
        "description": r["Description"],
        "location_description": r["Location Description"] or None,
        "arrest": r["Arrest"] == "true",
        "domestic": r["Domestic"] == "true",
        "beat": r["Beat"],
        "district": r["District"] or None,
        "ward": num(r["Ward"], int),
        "community_area": num(r["Community Area"], int),
        "fbi_code": r["FBI Code"],
        "x_coordinate": num(r["X Coordinate"]),
        "y_coordinate": num(r["Y Coordinate"]),
        "year": int(r["Year"]),
        "updated_on": dt(r["Updated On"]),
        "latitude": lat,
        "longitude": lon,
        "location": {"type": "Point", "coordinates": [lon, lat]} if lat is not None else None,
    }

with open(path, newline="") as f:
    records = [to_record(r) for r in csv.DictReader(f)]
print(json.dumps(records[0], indent=2))
```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Chicago Crime Record
description: 'A single reported crime incident from the City of Chicago "Crimes -
  2001 to Present" dataset.

  Property names follow the Chicago Data Portal (Socrata) API field names; values
  are typed

  (booleans, numbers, ISO 8601 date-times, GeoJSON) rather than the all-string CSV
  export.

  '
type: object
properties:
  id:
    description: Unique identifier for the record (CSV column "ID").
    type: string
    pattern: ^[0-9]+$
    x-jsonld-id: http://purl.org/dc/terms/identifier
  case_number:
    description: Chicago Police Department Records Division number, unique to the
      incident (CSV column "Case Number").
    type: string
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/caseNumber
  date:
    description: Date and local (America/Chicago) time when the incident occurred,
      sometimes a best estimate (CSV column "Date", converted from "MM/DD/YYYY hh:mm:ss
      AM" to ISO 8601 without offset).
    $ref: '#/$defs/LocalDateTime'
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/date
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  block:
    description: Partially redacted address, placing the incident on the same block
      as the actual address (CSV column "Block").
    type: string
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/block
  iucr:
    description: Illinois Uniform Crime Reporting code (CSV column "IUCR").
    type: string
    pattern: ^[0-9A-Z]{4}$
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/iucr
  primary_type:
    description: Primary description of the IUCR code (CSV column "Primary Type").
    type: string
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/primaryType
  description:
    description: Secondary description of the IUCR code (CSV column "Description").
    type: string
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/description
  location_description:
    description: Description of the location where the incident occurred (CSV column
      "Location Description").
    type:
    - string
    - 'null'
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/locationDescription
  arrest:
    description: Whether an arrest was made (CSV column "Arrest").
    type: boolean
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/arrest
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  domestic:
    description: Whether the incident was domestic-related as defined by the Illinois
      Domestic Violence Act (CSV column "Domestic").
    type: boolean
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/domestic
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  beat:
    description: Police beat where the incident occurred; the smallest police geographic
      area (CSV column "Beat").
    type: string
    pattern: ^[0-9]{4}$
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/beat
  district:
    description: Police district where the incident occurred (CSV column "District").
    type:
    - string
    - 'null'
    pattern: ^[0-9]{3}$
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/district
  ward:
    description: City Council district (ward) where the incident occurred (CSV column
      "Ward").
    type:
    - integer
    - 'null'
    minimum: 1
    maximum: 50
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/ward
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#integer
  community_area:
    description: Community area where the incident occurred; Chicago has 77 community
      areas (CSV column "Community Area").
    type:
    - integer
    - 'null'
    minimum: 1
    maximum: 77
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/communityArea
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#integer
  fbi_code:
    description: Crime classification per the FBI National Incident-Based Reporting
      System (CSV column "FBI Code").
    type: string
    pattern: ^[0-9]{2}[A-Z]?$
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/fbiCode
  x_coordinate:
    description: X coordinate of the (shifted) incident location in State Plane Illinois
      East NAD 1983 projection, US feet (EPSG:3435) (CSV column "X Coordinate").
    type:
    - number
    - 'null'
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/xCoordinate
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#decimal
  y_coordinate:
    description: Y coordinate of the (shifted) incident location in State Plane Illinois
      East NAD 1983 projection, US feet (EPSG:3435) (CSV column "Y Coordinate").
    type:
    - number
    - 'null'
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/yCoordinate
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#decimal
  year:
    description: Year the incident occurred (CSV column "Year").
    type: integer
    minimum: 2001
    x-jsonld-id: https://platform.i-guide.io/def/chicago-crime/year
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#gYear
  updated_on:
    description: Date and local time the record was last updated (CSV column "Updated
      On").
    $ref: '#/$defs/LocalDateTime'
    x-jsonld-id: http://purl.org/dc/terms/modified
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  latitude:
    description: WGS 84 latitude of the (shifted) incident location (CSV column "Latitude").
    type:
    - number
    - 'null'
    minimum: -90
    maximum: 90
    x-jsonld-id: http://www.w3.org/2003/01/geo/wgs84_pos#lat
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#decimal
  longitude:
    description: WGS 84 longitude of the (shifted) incident location (CSV column "Longitude").
    type:
    - number
    - 'null'
    minimum: -180
    maximum: 180
    x-jsonld-id: http://www.w3.org/2003/01/geo/wgs84_pos#long
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#decimal
  location:
    description: Incident location as a GeoJSON Point (WGS 84 longitude, latitude).
      The CSV column "Location" carries the same point as WKT, e.g. "POINT (-87.654831089
      41.772324861)".
    oneOf:
    - $ref: '#/$defs/Point'
    - type: 'null'
    x-jsonld-id: http://www.opengis.net/ont/geosparql#hasGeometry
    x-jsonld-type: '@json'
required:
- id
- case_number
- date
- iucr
- primary_type
- arrest
- domestic
- beat
- fbi_code
- year
$defs:
  LocalDateTime:
    type: string
    pattern: ^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}$
  Point:
    type: object
    required:
    - type
    - coordinates
    properties:
      type:
        const: Point
      coordinates:
        type: array
        minItems: 2
        maxItems: 2
        items:
          type: number
x-jsonld-prefixes:
  dct: http://purl.org/dc/terms/
  ccr: https://platform.i-guide.io/def/chicago-crime/
  xsd: http://www.w3.org/2001/XMLSchema#
  wgs84: http://www.w3.org/2003/01/geo/wgs84_pos#
  geo: http://www.opengis.net/ont/geosparql#

```

Links to the schema:

* YAML version: [schema.yaml](https://raw.githubusercontent.com/I-GUIDE/ogc-bblock-iguide/undefined/build/annotated/bbr/chicagoCrime/schema.json)
* JSON version: [schema.json](https://raw.githubusercontent.com/I-GUIDE/ogc-bblock-iguide/undefined/build/annotated/bbr/chicagoCrime/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "id": "dct:identifier",
    "case_number": "ccr:caseNumber",
    "date": {
      "@id": "ccr:date",
      "@type": "xsd:dateTime"
    },
    "block": "ccr:block",
    "iucr": "ccr:iucr",
    "primary_type": "ccr:primaryType",
    "description": "ccr:description",
    "location_description": "ccr:locationDescription",
    "arrest": {
      "@id": "ccr:arrest",
      "@type": "xsd:boolean"
    },
    "domestic": {
      "@id": "ccr:domestic",
      "@type": "xsd:boolean"
    },
    "beat": "ccr:beat",
    "district": "ccr:district",
    "ward": {
      "@id": "ccr:ward",
      "@type": "xsd:integer"
    },
    "community_area": {
      "@id": "ccr:communityArea",
      "@type": "xsd:integer"
    },
    "fbi_code": "ccr:fbiCode",
    "x_coordinate": {
      "@id": "ccr:xCoordinate",
      "@type": "xsd:decimal"
    },
    "y_coordinate": {
      "@id": "ccr:yCoordinate",
      "@type": "xsd:decimal"
    },
    "year": {
      "@id": "ccr:year",
      "@type": "xsd:gYear"
    },
    "updated_on": {
      "@id": "dct:modified",
      "@type": "xsd:dateTime"
    },
    "latitude": {
      "@id": "wgs84:lat",
      "@type": "xsd:decimal"
    },
    "longitude": {
      "@id": "wgs84:long",
      "@type": "xsd:decimal"
    },
    "location": {
      "@id": "geo:hasGeometry",
      "@type": "@json"
    },
    "dct": "http://purl.org/dc/terms/",
    "ccr": "https://platform.i-guide.io/def/chicago-crime/",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "wgs84": "http://www.w3.org/2003/01/geo/wgs84_pos#",
    "geo": "http://www.opengis.net/ont/geosparql#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://raw.githubusercontent.com/I-GUIDE/ogc-bblock-iguide/undefined/build/annotated/bbr/chicagoCrime/context.jsonld)

## Sources

* [I-GUIDE Platform - Chicago Crime data 2026](https://platform.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627)
* [Chicago Data Portal - Crimes - 2001 to Present](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/I-GUIDE/ogc-bblock-iguide](https://github.com/I-GUIDE/ogc-bblock-iguide)
* Path: `_sources/chicagoCrime`

