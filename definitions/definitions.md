---
title: API Property Definitions
---
# Overview

This page describes all the common properties used in Airbus defence and Space Géo. It specifically describes:
- The property name
- The property meaning (description)
- The property value format when used in a response
- The property value format when used in a request

Whenever possible, property names, meaning and values are reused from:
- [GeoJSON] RFC 7946 published in august 2016.
- [OGC OpenSearch Extension for Earth Observation, version 1.0.0]
- [OpenSearch Geo extension (Draft)]
- [OpenSearch SRU extension (Draft)]

# Properties

| Name | Description | Response format | Request format
| --- | --- | --- | --- 
| acquisitionDate | Date at which the image was acquired by the satellite. | [W3C ISO 8601] subset: `1997-07-16`| [W3C ISO 8601] subset [range] : `[2016-2017]`
| bbox | The bounding box of a geometry as described in the [GeoJSON specification](https://tools.ietf.org/html/rfc7946#section-5). | An array of number: `[-10.0, -10.0, 10.0, 10.0]` | An array of number: `[-10.0, -10.0, 10.0, 10.0]`. When used in a query parameter, the brackets are omitted: `-10.0, -10.0, 10.0, 10.0`
| sourceId | Identifier of the segment acquired by the satellite which is used to produce products | String : `DS_PHR1A_201704141032506_FR1_PX_W001N07_0418_03164` | String

# Annexes

## Mathematical notation for ranges and sets

Mathematical notation for ranges and sets to define intervals uses the following syntax: 
- n1 equal to property = n1
- {n1,n2,…} equals to property=n1 OR property=n2 OR …
- [n1,n2] equals to n1 <= property <= n2
- [n1,n2[ equals to n1 <= property < n2
- ]n1,n2[ equals to n1 < property < n2
- ]n1,n2] equals to n1 < property  <= n2
- [n1 equals to n1 <= property
- ]n1 equals to n1 < property
- n2] equals to property <= n2
- n2[ equals to property < n2

__Exemples__

`cloudCover` property set to `10[` in an image search request means that we would like to retrieve images with a cloudCover property strictly less to 10%.

`acquisitionDate` property set to `[2016-01-01T00:00+00:00,2017-01-01T00:00+00:00[` in an image search request means that we would like to retrieve images acquired in year 2016 only.

Reference: [OGC OpenSearch Extension for Earth Observation, version 1.0.0]

<!-- ......................................................................

                                     Links 

     ...................................................................... -->

<!-- Internal links -->
[range]: #Mathematical notation for ranges

<!-- External links -->
[GeoJSON]: https://tools.ietf.org/html/rfc7946
[OGC OpenSearch Extension for Earth Observation, version 1.0.0]: http://docs.opengeospatial.org/is/13-026r8/13-026r8.html
[OpenSearch Geo extension (Draft)]: http://www.opensearch.org/Specifications/OpenSearch/Extensions/Geo/1.0/Draft_2
[OpenSearch SRU extension (Draft)]: http://www.opensearch.org/Community/Proposal/Specifications/OpenSearch/Extensions/SRU/1.0/Draft_1
[W3C ISO 8601]: https://www.w3.org/TR/NOTE-datetime


