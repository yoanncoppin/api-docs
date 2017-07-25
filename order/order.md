The ordering API allows you to order archive images and/or task the satellites for new acquisitions, from your own portal. This guide will show you step by step how to order an archive image. For more information refer to the online reference documentation : <https://airbusgeo.github.io/geoapi-viewer/?url=https://airbusgeo.github.io/api-docs/order.yaml>


In this scenario, we want to order a precise image of Toulouse (France). To do so, we will order a pleiades archive image.

# Step 1
First of all, you have to get an authentication token to be able to send requests to other endpoints of the ordering API.
Execute the following command:
```shell
curl -X POST -H "content-Type: application/x-www-form-urlencoded" -d  \
'client_id=xxxxxxxx&client_secret=xxxxxxxxx&scope=order.api&grant_type=client_credentials' \
-k "https://is-qual.geoapi-airbusds.com:9443/ADS-ID/core/connect/token" 
```
where xxxxxx must be replaced by the values sent to you in a crypted file.

```json
{
  "access_token": "eyJ0eXAiOiJKV1Qxxxxxxxxxxxxxxxxxxxxx",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

Once you got a token you can start using the GeoConnector API via the following endpoint: <https://order-qual.geoapi-airbusds.com:8443>. Insert the token ("access_token" value) in the Authorization field of each request you send to the Order API. 
Note that the header of the requests must also contain the field X-ADS-Username which must be encoded in base64.
To encode the username:
```shell
echo -n xxxxx| base64
eHh4eHg=
```
(replace the xxxxx by your the username you in the crypted file your received by email)

Requests header:

```shell
export MY_TOKEN=***INSERT YOUR TOKEN HERE***
export MY_USERNAME=***INSERT THE OUTPUT VALUE OF THE LAST COMMAND LINE***
```

- Field name : Authorization (value = token)
- Field name : X-ADS-Username (value in the crypted file you received by email)


# Step 2
You might want to get the remaining credit on your contract. The following curl command is optional if you already know your contractId. This command is also used to get to know the remaining credit on your contract(s)
If you do not, send a GET /me/contracts to the server:
```shell
curl -X GET \
  -k https://order-qual.geoapi-airbusds.com:8443/api/v1/me/contracts \
  -H "Authorization: $MY_TOKEN" \
  -H "X-ADS-Username: $MY_USERNAME"
```

You would get a response similar to:
```json
{
 "contracts": [
 	{
 "contractId": "myContractID",
 "name": " myAccountName",
 "tradeAgreementUrl": "https://www.intelligence-airbusds.com/fr/4971-cgf",
 "creditRemaining": "100000.0",
 "currency": "EUR",
 "language": "fr"
 }
 ]
} 
```

Then, export your contractId:
```shell
export CONTRACT_ID=***INSERT THE GIVEN CONTRACTID HERE***
```

Note that there might be several contracts returned. You have to choose one according to which product type you want to order.

# Step 3
Calling the following request is optional but the export is not. You should use it to know which product types you can order. Once you have chosen it, you can export its productTypeId:
```shell
curl -X GET \
	-k https://order-qual.geoapi-airbusds.com:8443/api/v1/productTypes \
	-H "Authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "X-ADS-Username: $MY_USERNAME"
```

```json
{"productTypes": [
      {
      "productTypeId": "DMCArchive22",
      "technicalProduct":       {
         "id": "DMCArchive22",
         "label": "DMC",
         "source":          {
            "id": "archive",
            "label": "archive"
         },
         "sensor":          {
            "id": "dmc",
            "label": "DMC"
         },
         "target":          {
            "id": "standard22m",
            "label": "Standard"
         },
         "licences": null
      },
      "minimumOrderAreaApplied": 0,
      "descriptionUrl": "http://www.intelligence-airbusds.com/en/84-dmc-constellation"
   },
      {
      "productTypeId": "PleiadesArchiveMono",
      "technicalProduct":       {
         "id": "PleiadesArchiveMono",
         "label": "Pleiades 0.5-m",
         "source":          {
            "id": "archive",
            "label": "archive"
         },
         "sensor":          {
            "id": "pleiades",
            "label": "PlÃ©iades"
         },
         "target":          {
            "id": "mono",
            "label": "Mono"
         },
         "licences": null
      },
      "minimumOrderAreaApplied": 25,
      "descriptionUrl": "https://www.intelligence-airbusds.com/en/3027-pleiades-50-cm-resolution-products"
   }
]}
```

Select the product type you want to order:

```shell
export PRODUCTTYPE_ID=***INSERT THE productOfferID of the wanted productType***
```
Note that in this example, we choose "PleiadesArchiveMono"

#Step 4

Once your know which product type you want to order, you may want to look for archive images in our catalogue to find the one you will order. You will then use the search API. 
The search API will return numerous properties of the images (identifier, footprint...). You will need some of them in the following steps.
To use this API correctly and know how to get your "MY_APIKEY", please refer to its documentation <https://airbusgeo.github.io/api-docs/search/search.html>.

We chose the following coordinates because it covers the city of Toulouse, which is our area of interest. We also chose the pleiades satellites images for very precise images. A random date was chosen, so you will only retreive one image.
```shell
curl -X POST -H "Authorization: APIKEY $MY_APIKEY" -H "Content-Type: 
application/json" -d '{
   "geometry": "POLYGON((1.3348388671875 43.66240779260363,
1.5641784667968752 43.66240779260363, 1.5641784667968752 43.516190595612755, 1.3348388671875 43.516190595612755, 1.3348388671875 43.66240779260363))",
   "constellation": [ "PLEIADES" ],
   "acquisitionDate": "[2017-01-24T00:00:00,2017-01-24T23:59:59]"
}' "https://search-qual.geoapi-airbusds.com/api/v1/search"
```

Example:
```json
{
   "startIndex": 1,
   "itemsPerPage": 25,
   "totalResults": 1,
   "query": {
     "geometry": "POLYGON((1.3348388671875 43.66240779260363,
1.5641784667968752 43.66240779260363, 1.5641784667968752 43.516190595612755, 1.3348388671875 43.516190595612755, 1.3348388671875 43.66240779260363))",
     "constellation": [
       "PLEIADES"
     ],
     "acquisitionDate": "[2017-01-24T00:00:00,2017-01-24T23:59:59]"
   },
   "type": "FeatureCollection",
   "features": [
     {
       "type": "Feature",
       "properties": {
         "viewingAngleAlongTrack": -11.14605527365686,
         "incidenceAngle": 32.29047084960579,
         "sourceId": 
"DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664",
         "acquisitionDate": "2017-01-24T10:37:51.229Z",
         "constellation": "PLEIADES",
         "provider": "Airbus Defence and Space",
         "viewingAngleAcrossTrack": -27.0263716551796,
         "cloudCover": 0,
         "satellite": "PHR1A",
         "creationDate": "2017-06-10T20:19:30.810328Z",
         "resolution": 0.5,
         "productName": "Pléiades 0.5-m"
       },
       "geometry": {
         "type": "Polygon",
         "coordinates": [
           [
             [
               1.257353827342168,
               43.51499992670583
             ],
             [
               1.256790254518078,
               43.54711045952346
             ],
             [
               1.255514795769583,
               43.71489050656499
             ],
             [
               1.384754739081241,
               43.7029560489154
             ],
             [
               1.45009224373546,
               43.69639005169051
             ],
             [
               1.584824320385018,
               43.68207395444014
             ],
             [
               1.584816585304249,
               43.66084996806644
             ],
             [
               1.584249661225448,
               43.629278017562
             ],
             [
               1.584312238288345,
               43.61867177220752
             ],
             [
               1.583972915011884,
               43.59767029007774
             ],
             [
               1.584130008203344,
               43.58704871537454
             ],
             [
               1.583680879320572,
               43.55564297303177
             ],
             [
               1.583861547553116,
               43.50292413472972
             ],
             [
               1.583781598514204,
               43.48552351216974
             ],
             [
               1.521843231208238,
               43.49169073436082
             ],
             [
               1.361200893781275,
               43.50646455923665
             ],
             [
               1.257353827342168,
               43.51499992670583
             ]
           ]
         ]
       },
       "id": 
"bcfdb18a-14e2-47f7-9fae-dce82f8250b6::4cb18a03-e95a-4bc8-aaed-66aaf9032807",
       "bbox": [
         1.255514795769583,
         43.48552351216974,
         1.584824320385018,
         43.71489050656499
       ],
       "thumbnail": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?size=SMALL",
       "quicklooks": [
         {
           "size": "MEDIUM",
           "projection": "http://www.opengis.net/def/crs/EPSG/0/4326",
           "image": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?projection=EPSG:4326&size=MEDIUM"
         },
         {
           "size": "LARGE",
           "projection": null,
           "image": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?size=LARGE"
         },
         {
           "size": "MEDIUM",
           "projection": null,
           "image": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?size=MEDIUM"
         },
         {
           "size": "MEDIUM",
           "projection": "http://www.opengis.net/def/crs/EPSG/0/3857",
           "image": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?projection=EPSG:3857&size=MEDIUM"
         },
         {
           "size": "SMALL",
           "projection": null,
           "image": 
"https://search-qual.geoapi-airbusds.com/api/v1/productTypes/PHR_0_5/products/DS_PHR1A_201701241037483_FR1_PX_E001N43_0615_01664?size=SMALL"
         }
       ]
     }
   ]
}
```
We will then use the value of the sourceId field.

WARNING: For now, we use WKT format to form polygons in the search API. We will soon let GeoJson format available.


# Step 5
This step is used to retrieve your product type's options you can apply for your order:
```shell
curl -X POST \
	-k https://order-qual.geoapi-airbusds.com:8443/api/v1/productTypes/$PRODUCTTYPE_ID/options \
	-H "Authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "X-ADS-Username: $MY_USERNAME" \
  -d '{ "aoi": [ { "polygonId": 1, "geometry": { "type": "Polygon", "coordinates": [[[1.3348388671875,43.66240779260363],[1.5641784667968752,43.66240779260363],[1.5641784667968752,43.516190595612755],[1.3348388671875,43.516190595612755],[1.3348388671875,43.66240779260363]]] } } ] }'
```

Note that you should replace the coordinates by the ones you chose to search your picture in the Search API.

You would get a response similar to:
```json
{"availableOptions": [
      {
      "category": "technical_product",
      "constraint": null,
      "label": "PROCESSING_LEVEL",
      "name": "processing_level",
      "type": "list",
      "values":       [
                  {
            "id": "ortho",
            "label": "Ortho"
         },
                  {
            "id": "projected",
            "label": "Projected"
         },
                  {
            "id": "primary",
            "label": "Primary"
         }
      ],
      "defaultValue": "ortho",
      "range": null,
      "format": null,
      "mandatory": "true"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": false,
         "filter": "processing_level:ortho"
      },
      "label": "ORTHORECTIFICATION_DEM_REFERENCE",
      "name": "dem",
      "type": "list",
      "values":       [
                  {
            "id": "best_available",
            "label": "Best Available"
         },
                  {
            "id": "srtm",
            "label": "SRTM"
         },
                  {
            "id": "globe",
            "label": "Globe"
         }
      ],
      "defaultValue": "best_available",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": true,
         "filter": "processing_level:primary"
      },
      "label": "SPECTRAL_PROCESSING",
      "name": "spectral_processing",
      "type": "list",
      "values":       [
                  {
            "id": "pansharpened_natural_color",
            "label": "Pansharpened 50cm 3-band, Natural Color"
         },
                  {
            "id": "panchromatic",
            "label": "Panchromatic 50cm"
         },
                  {
            "id": "pansharpened",
            "label": "Pansharpened 50cm 4-band"
         },
                  {
            "id": "multispectral",
            "label": "Multispectral 2m 4-band"
         },
                  {
            "id": "pansharpened_false_color",
            "label": "Pansharpened 50cm 3-band, False Color"
         },
                  {
            "id": "bundle",
            "label": "Bundle: Panchromatic 50cm + Multispectral 2m 4-band"
         }
      ],
      "defaultValue": "pansharpened",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": false,
         "filter": "processing_level:primary"
      },
      "label": "SPECTRAL_PROCESSING",
      "name": "spectral_processing_primary",
      "type": "list",
      "values":       [
                  {
            "id": "pansharpened_natural_color",
            "label": "Pansharpened 50cm 3-band, Natural Color"
         },
                  {
            "id": "panchromatic",
            "label": "Panchromatic 50cm"
         },
                  {
            "id": "pansharpened",
            "label": "Pansharpened 50cm 4-band"
         },
                  {
            "id": "multispectral",
            "label": "Multispectral 2m 4-band"
         },
                  {
            "id": "pansharpened_false_color",
            "label": "Pansharpened 50cm 3-band, False Color"
         },
                  {
            "id": "bundle",
            "label": "Bundle: Panchromatic 50cm + Multispectral 2m 4-band"
         }
      ],
      "defaultValue": "bundle",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": false,
         "filter": "processing_level:primary"
      },
      "label": "FORMAT",
      "name": "image_format_primary",
      "type": "list",
      "values":       [
                  {
            "id": "dimap_jpeg2000_optimized",
            "label": "DIMAP - Optimized JPEG 2000 "
         },
                  {
            "id": "dimap_jpeg2000_regular",
            "label": "DIMAP - Regular JPEG 2000 "
         },
                  {
            "id": "dimap_geotiff",
            "label": "DIMAP - GeoTIFF"
         }
      ],
      "defaultValue": "dimap_jpeg2000_regular",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": true,
         "filter": "processing_level:primary"
      },
      "label": "FORMAT",
      "name": "image_format",
      "type": "list",
      "values":       [
                  {
            "id": "dimap_jpeg2000_optimized",
            "label": "DIMAP - Optimized JPEG 2000 "
         },
                  {
            "id": "dimap_jpeg2000_regular",
            "label": "DIMAP - Regular JPEG 2000 "
         },
                  {
            "id": "dimap_geotiff",
            "label": "DIMAP - GeoTIFF"
         },
                  {
            "id": "nitf_jpeg2000_regular",
            "label": "NITF - Regular JPEG 2000 "
         },
                  {
            "id": "nitf_geotiff",
            "label": "NITF - GeoTIFF"
         }
      ],
      "defaultValue": "dimap_jpeg2000_regular",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   },
      {
      "category": "technical_product",
      "constraint": null,
      "label": "PIXEL_CODING",
      "name": "pixel_coding",
      "type": "list",
      "values":       [
                  {
            "id": "8bits",
            "label": "8 bits (JPEG 2000 / GeoTIFF) "
         },
                  {
            "id": "12bits",
            "label": "12 bits (JPEG 2000) / 16 bits (GeoTIFF)"
         }
      ],
      "defaultValue": "12bits",
      "range": null,
      "format": null,
      "mandatory": "true"
   },
      {
      "category": "technical_product",
      "constraint": null,
      "label": "RADIOMETRIC_PROCESSING",
      "name": "radiometric_processing",
      "type": "list",
      "values":       [
                  {
            "id": "basic",
            "label": "Basic"
         },
                  {
            "id": "reflectance",
            "label": "Reflectance"
         },
                  {
            "id": "display",
            "label": "Display"
         }
      ],
      "defaultValue": "basic",
      "range": null,
      "format": null,
      "mandatory": "true"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": false,
         "filter": "processing_level:projected"
      },
      "label": "ELEVATION",
      "name": "elevation",
      "type": "double",
      "values": null,
      "defaultValue": " ",
      "range":       [
         -100,
         10000
      ],
      "format": null,
      "mandatory": "false"
   },
      {
      "category": "technical_product",
      "constraint": null,
      "label": "AREA_COMPUTE_MODEL",
      "name": "area_compute_model",
      "type": "list",
      "values":       [
                  {
            "id": "coverage",
            "label": "Coverage"
         },
                  {
            "id": "temporal_serie",
            "label": "TemporalSerie"
         }
      ],
      "defaultValue": "coverage",
      "range": null,
      "format": null,
      "mandatory": "false"
   },
      {
      "category": "technical_product",
      "constraint": null,
      "label": "PRIORITY",
      "name": "priority",
      "type": "list",
      "values":       [
                  {
            "id": "standard",
            "label": "Standard"
         },
                  {
            "id": "rush",
            "label": "Rush (Please select FTP delivery - No Quality Checking)"
         }
      ],
      "defaultValue": "standard",
      "range": null,
      "format": null,
      "mandatory": "true"
   },
      {
      "category": "technical_product",
      "constraint": null,
      "label": "LICENSE",
      "name": "license",
      "type": "list",
      "values":       [
                  {
            "id": "eula_5",
            "label": "EULA (up to 5 affiliated end users)"
         },
                  {
            "id": "eula_6_10",
            "label": "Multi (6 to 10 affiliated end users)"
         },
                  {
            "id": "eula_11_30",
            "label": "Multi (11 to 30 affiliated end users)"
         }
      ],
      "defaultValue": "eula_5",
      "range": null,
      "format": null,
      "mandatory": "true"
   },
      {
      "category": "technical_product",
      "constraint":       {
         "reject": true,
         "filter": "processing_level:primary"
      },
      "label": "Projection (GDLIB code)",
      "name": "projection_1",
      "type": "list",
      "values":       [
                  {
            "id": "2154",
            "label": "2154 RGF93/Lambert-93 (FR.) - PROJECTED"
         },
                  {
            "id": "23032",
            "label": "23032 ED50/UTM 32N (EU. - 6E to 12E) - PROJECTED"
         },
                  {
            "id": "25832",
            "label": "25832 ETRS89/UTM 32N (EUR. - 6E to 12E) - PROJECTED"
         },
                  {
            "id": "27564",
            "label": "27564 NTF (Paris)/Lambert Corse (FR. - Corsica) - PROJECTED"
         },
                  {
            "id": "27574",
            "label": "27574 NTF (Paris)/Lambert zone IV (FR. - Corsica) - PROJECTED"
         },
                  {
            "id": "3003",
            "label": "3003 Monte Mario/Italy 1 (IT. - W. of 12E) - PROJECTED"
         },
                  {
            "id": "3035",
            "label": "3035 ETRS89/LAEA Europe (EUR.) - PROJECTED"
         },
                  {
            "id": "32232",
            "label": "32232 WGS72/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
         },
                  {
            "id": "32432",
            "label": "32432 WGS72BE/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
         },
                  {
            "id": "32631",
            "label": "32631 WGS84/UTM 31N (N. hemisphere - 0E to 6E) - PROJECTED"
         },
                  {
            "id": "32632",
            "label": "32632 WGS84/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
         },
                  {
            "id": "32633",
            "label": "32633 WGS84/UTM 33N (N. hemisphere - 12E to 18E) - PROJECTED"
         },
                  {
            "id": "3349",
            "label": "3349 WGS84/PDC Mercator (Pacific Ocean 99E. to 70W.) - PROJECTED"
         },
                  {
            "id": "3395",
            "label": "3395 WGS84/World Mercator (World - between 80S. and 84N.) - PROJECTED"
         },
                  {
            "id": "3857",
            "label": "3857 WGS84/Web Mercator (World - between 85S. and 85N.) - PROJECTED"
         },
                  {
            "id": "4326",
            "label": "4326 WGS 1984 - GEOGRAPHIC"
         }
      ],
      "defaultValue": "32632",
      "range": null,
      "format": null,
      "mandatory": "constraint"
   }
]}
```

For each option, if the field named "mandatory" is "true", you must add if to your orderRequest as following:
```json
{ 
	"key": ***THE VALUE OF THE "NAME" FIELD OF THE WANTED OPTION***,
	"value": ***THE "id" OF THE WANTED VALUE***
}
```

Note that if you are a begginer, you should choose, as "value", the value of the "defaultValue" field. Those concatenated values will be pasted in the optionsPerProductType.options field.


# Step 6

Using the image properties returned by the search API (step 1), you can use the order API to get the price of the image, using the following POST request:
```shell
curl -X POST \
  -k https://order-qual.geoapi-airbusds.com:8443/api/v1/prices \
  -H "authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "x-ads-username: $MY_USERNAME" \
  -d '{
   "aoi": [   {
      "id": 1,
      "name": "test_for_partners",
      "geometry":       {
         "type": "Polygon",
         "coordinates": [         [
                        [
               8.362239801370935,
               42.27393795330642
            ],
                        [
               9.07687956225646,
               42.16948996313908
            ],
                        [
               8.898113694237654,
               41.645365338986835
            ],
                        [
               8.189353632881046,
               41.748912927959445
            ],
                        [
               8.362239801370935,
               42.27393795330642
            ]
         ]]
      }
   }],
   "voucherCodes": [],
   "secondaryMarket": "AGRI_ENV",
   "items": [   {
      "productTypeId": "'$PRODUCTTYPE_ID'",
      "aoiId": 1,
      "datastripIds": ["40582651209160926432R5"]
   }],
   "contractId": "'$CONTRACT_ID'",
   "optionsPerProductType": [   {
      "licence": "eula_5",
      "options":       [
                  {
            "value": "dimap_geotiff",
            "key": "image_format"
         },
                  {
            "value": "DDPORTAL",
            "key": "Account"
         },
                  {
            "value": "32632",
            "key": "projection_1"
         },
                  {
            "value": "level_3",
            "key": "processing_level"
         },
        {
        	"value": "eula_5",
        	"key": "license"
        }
      ],
      "productType":       {
         "productTypeId": "'$PRODUCTTYPE_ID'"
      }
   }],
   "customerReference": "The order reference that will appear in invoicing",
   "primaryMarket": "AGRI",
   "options": [   {
      "value": "network",
      "key": "deliveryType"
   }]
}'
```

# Step 7

Note that you should replace the coordinates by the ones you want, the productTypeId by the one of the product type you want and the datastripId by the searchAPI's resquest sourceId. The optionsPerProductType must be filled by
all the mandatory fields from the productTypes options. For instance, in the given productTypes options example, you should select the name (processing_level in that cas) as key and the wanted value id ("ortho" for instance) as value.
The parameter field geometry is given by the search API.

If the request above is sent to the endpoint /prices, you will get a response containing the price of the image, similar to the following one:
```json
{
            "contractId": "myContractID",
            "currency": "EUR",
            "discountPercentage": 0.0,
            "discountValue": 0.0,
            "productTypes" : [{
              "amount": 200.0,
              "areaKm2" : 20.0,
              "discountPercentage": 0.0,
              "discountValue": 0.0,
              "price": 200.0,
              "productTypeId": "SPOTArchive10Color",
              "items": [
                { "dataStripIds": ["40582651209160926432R5"] }
              ]
            }],
            "totalAmountCurrency": 200.0            
}
```

# Step 7

If the step 6 request is sent to the endpoint /orders instead of /prices, you can use the order API to order the image, using the same POST request:
```shell
curl -X POST \
  -k https://order-qual.geoapi-airbusds.com:8443/api/v1/orders \
  -H "authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "x-ads-username: $MY_USERNAME" \
  -d '{
   "aoi": [   {
      "id": 1,
      "name": "test_for_partners",
      "geometry":       {
         "type": "Polygon",
         "coordinates": [         [
                        [
               8.362239801370935,
               42.27393795330642
            ],
                        [
               9.07687956225646,
               42.16948996313908
            ],
                        [
               8.898113694237654,
               41.645365338986835
            ],
                        [
               8.189353632881046,
               41.748912927959445
            ],
                        [
               8.362239801370935,
               42.27393795330642
            ]
         ]]
      }
   }],
   "voucherCodes": [],
   "secondaryMarket": "AGRI_ENV",
   "items": [   {
      "productTypeId": "'$PRODUCTTYPE_ID'",
      "aoiId": 1,
      "datastripIds": ["40582651209160926432R5"]
   }],
   "contractId": "'$CONTRACT_ID'",
   "optionsPerProductType": [   {
      "licence": "eula_5",
      "options":       [
                  {
            "value": "dimap_geotiff",
            "key": "image_format"
         },
                  {
            "value": "DDPORTAL",
            "key": "Account"
         },
                  {
            "value": "32632",
            "key": "projection_1"
         },
                  {
            "value": "level_3",
            "key": "processing_level"
         },
        {
        	"value": "eula_5",
        	"key": "license"
        }
      ],
      "productType":       {
         "productTypeId": "'$PRODUCTTYPE_ID'"
      }
   }],
   "customerReference": "The order reference that will appear in invoicing",
   "primaryMarket": "AGRI",
   "options": [   {
      "value": "network",
      "key": "deliveryType"
   }]
}'
```

The image will be produced and delivered to you. You will get a response similar to the following one:
```json
{
	"salesOrderId" : "ORDER_UNIQUE_IDENTIFIER"
}
```
