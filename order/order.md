The ordering API allows you to order archive images and/or task the satellites for new acquisitions, from your own portal. This guide will show you step by step how to order an archive image. For more information refer to the online reference documentation : <https://airbusgeo.github.io/geoapi-viewer/?url=https://airbusgeo.github.io/api-docs/order.yaml>

# Step 1
First of all, you want to look for archive images in our catalogue in order to find the one you will order. You will then use the search API: <https://airbusgeo.github.io/api-docs/search/search.html>. 
The search API will return numerous properties of the images (identifier, footprint...) that you can use in the following steps.

Example:
```json
{
   "totalResults" : 1,
   "itemsPerPage" : 10,
   "type" : "FeatureCollection",
   "features" : [
      {
         "properties" : {
            "sourceId" : "DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140",
            "satellite" : "SPOT7",
            "productName" : "SPOT 1.5-m",
            "provider" : "Airbus Defence and Space",
            "cloudCover" : 1,
            "incidenceAngle" : 19.9324257673857,
            "constellation" : "SPOT",
            "acquisitionDate" : "2016-07-29T10:38:18.749Z",
            "resolution" : 1.5,
            "viewingAngleAcrossTrack" : 17.909748,
            "viewingAngleAlongTrack" : 17.842472
         },
         "id" : "ea1575c9-a4e5-4748-9e1b-fbc37d7286e9::5f08c566-0c5e-4baf-bf66-05d6c530aaf5",
         "thumbnail" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?size=SMALL",
         "geometry" : {
            "coordinates" : [
               [
                  [
                     1.00409636794572,
                     43.7046518627418
                  ],
                  [
                     1.83963153029114,
                     43.6915345718161
                  ],
                  [
                     1.84076579791152,
                     43.3869345568225
                  ],
                  [
                     1.0028176461841,
                     43.4014298007982
                  ],
                  [
                     1.00409636794572,
                     43.7046518627418
                  ]
               ]
            ],
            "type" : "Polygon"
         },
         "bbox" : [
            1.0028176461841,
            43.3869345568225,
            1.84076579791152,
            43.7046518627418
         ],
         "quicklooks" : [
            {
               "size" : "SMALL",
               "projection" : null,
               "image" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?size=SMALL"
            },
            {
               "projection" : null,
               "image" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?size=MEDIUM",
               "size" : "MEDIUM"
            },
            {
               "projection" : "http://www.opengis.net/def/crs/EPSG/0/4326",
               "image" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?projection=EPSG:4326&size=MEDIUM",
               "size" : "MEDIUM"
            },
            {
               "size" : "LARGE",
               "image" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?size=LARGE",
               "projection" : null
            },
            {
               "projection" : "http://www.opengis.net/def/crs/EPSG/0/3857",
               "image" : "https://search.geoapi-airbusds.com/api/v1/productTypes/SPOT_1_5/products/DS_SPOT7_201607291038129_FR1_FR1_FR1_FR1_E001N44_01140?projection=EPSG:3857&size=MEDIUM",
               "size" : "MEDIUM"
            }
         ],
         "type" : "Feature"
      }
   ],
   "startIndex" : 1,
   "query" : {
      "cloudCover" : "10]",
      "incidenceAngle" : "20]",
      "acquisitionDate" : "[2016-07-01,2016-07-31T23:59:59]",
      "constellation" : [
         "SPOT"
      ],
      "count" : 10,
      "sensorType" : "optical",
      "bbox" : [
         1.18,
         43.52,
         1.25,
         43.56
      ],
      "startPage" : 1
   }
}
```


# Step 2
In order to use the ordering API you must have a token. Send a POST request to : <https://is-qual.geoapi-airbusds.com:9443/ADS-ID/core/connect/token>. The body of the request must contain:
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

# Step 3
This step is optional. You might want to get the remaining credit on your contract. In this case, you send a GET /me/contracts to the server:
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

# Step 4
This step is optional. It is used to retrieve your productTypes' list:
```shell
curl -X GET \
	-k https://order-qual.geoapi-airbusds.com:8443/api/v1/productTypes \
	-H "Authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "X-ADS-Username: $MY_USERNAME"
```

```json
{"productOffers": [
      {
      "productOfferID": "DMCArchive22",
      "name": "UK-DMC2 22m",
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
      "catalogFilters": [      {
         "filter": "c5a7b14e-26ad-4ab8-bdc3-161a7206a7bb",
         "catalogID": "DDSearchAPI"
      }],
      "minimumOrderApplied": 0,
      "manualFeasibilityStudy": false,
      "candidateTemporalSerie": false,
      "description": "UK-DMC2 22m",
      "descriptionUrl": "http://www.intelligence-airbusds.com/en/84-dmc-constellation"
   },
   {
      "productOfferID": "SPOTArchive10Color",
      "name": "SPOT 10-m Colour",
      "technicalProduct":       {
         "id": "SPOTArchive10Color",
         "label": "SPOT 10-m Colour",
         "source":          {
            "id": "archive",
            "label": "archive"
         },
         "sensor":          {
            "id": "spot",
            "label": "SPOT"
         },
         "target":          {
            "id": "10color",
            "label": "10-m Colour"
         },
         "licences": null
      },
      "catalogFilters": [      {
         "filter": "b72c1de2-4ce9-4812-a96a-24fdd8ac39bd",
         "catalogID": "DDSearchAPI"
      }],
      "minimumOrderApplied": 3600,
      "manualFeasibilityStudy": false,
      "candidateTemporalSerie": false,
      "description": "SPOT Multispectral 10-meter imagery",
      "descriptionUrl": "https://www.intelligence-airbusds.com/en/143-spot-satellite-imagery"
   }
]}
```

Select the product type you want to order:

```shell
export PRODUCTTYPE_ID=***INSERT THE productOfferID of the wanted productType***
```
Note that in this example, we choose "SPOTArchive10Color"

# Step 5
This step is used to retrieve your productType's options:
```shell
curl -X POST \
	-k https://order-qual.geoapi-airbusds.com:8443/api/v1/productTypes/$PRODUCTTYPE_ID/options \
	-H "Authorization: $MY_TOKEN" \
  -H "content-type: application/json" \
  -H "X-ADS-Username: $MY_USERNAME" \
  -d '{ "aoi": [ { "polygonId": 1, "geometry": { "type": "Polygon", "coordinates": [[[8.492431640625004, 43.04480541304369],[9.591064453125002, 43.04480541304369], [9.591064453125002, 41.10419094457646], [8.492431640625004, 41.10419094457646],[8.492431640625004, 43.04480541304369]]] } } ] }'
```

Note that you should replace the coordinates by the ones you chose to search your picture in the Search API.

You would get a response similar to:
```json
{
	"availableOptions": [{
		"category": "technical_product",
		"constraint": null,
		"label": "PROCESSING_LEVEL",
		"name": "processing_level",
		"type": "list",
		"values": [{
			"id": "level_3",
			"label": "Processing Level 3"
		}, {
			"id": "level_2A",
			"label": "Processing Level 2A"
		}, {
			"id": "level_1A",
			"label": "Processing Level 1A"
		}],
		"defaultValue": "level_3",
		"range": null,
		"format": null,
		"mandatory": "true"
	}, {
		"category": "technical_product",
		"constraint": null,
		"label": "FORMAT",
		"name": "image_format",
		"type": "list",
		"values": [{
			"id": "dimap_geotiff",
			"label": "DIMAP - GeoTIFF"
		}],
		"defaultValue": "dimap_geotiff",
		"range": null,
		"format": null,
		"mandatory": "true"
	}, {
		"category": "technical_product",
		"constraint": null,
		"label": "LICENSE",
		"name": "license",
		"type": "list",
		"values": [{
			"id": "eula_5",
			"label": "EULA (up to 5 affiliated end users)"
		}, {
			"id": "eula_6_10",
			"label": "Multi (6 to 10 affiliated end users)"
		}, {
			"id": "eula_11_30",
			"label": "Multi (11 to 30 affiliated end users)"
		}],
		"defaultValue": "eula_5",
		"range": null,
		"format": null,
		"mandatory": "true"
	}, {
		"category": "technical_product",
		"constraint": {
			"reject": true,
			"filter": "processing_level:level_1A"
		},
		"label": "Projection (GDLIB code)",
		"name": "projection_1",
		"type": "list",
		"values": [{
			"id": "2154",
			"label": "2154 RGF93/Lambert-93 (FR.) - PROJECTED"
		}, {
			"id": "23032",
			"label": "23032 ED50/UTM 32N (EU. - 6E to 12E) - PROJECTED"
		}, {
			"id": "25832",
			"label": "25832 ETRS89/UTM 32N (EUR. - 6E to 12E) - PROJECTED"
		}, {
			"id": "27564",
			"label": "27564 NTF (Paris)/Lambert Corse (FR. - Corsica) - PROJECTED"
		}, {
			"id": "27574",
			"label": "27574 NTF (Paris)/Lambert zone IV (FR. - Corsica) - PROJECTED"
		}, {
			"id": "3003",
			"label": "3003 Monte Mario/Italy 1 (IT. - W. of 12E) - PROJECTED"
		}, {
			"id": "3035",
			"label": "3035 ETRS89/LAEA Europe (EUR.) - PROJECTED"
		}, {
			"id": "32232",
			"label": "32232 WGS72/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
		}, {
			"id": "32432",
			"label": "32432 WGS72BE/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
		}, {
			"id": "32631",
			"label": "32631 WGS84/UTM 31N (N. hemisphere - 0E to 6E) - PROJECTED"
		}, {
			"id": "32632",
			"label": "32632 WGS84/UTM 32N (N. hemisphere - 6E to 12E) - PROJECTED"
		}, {
			"id": "32633",
			"label": "32633 WGS84/UTM 33N (N. hemisphere - 12E to 18E) - PROJECTED"
		}, {
			"id": "3349",
			"label": "3349 WGS84/PDC Mercator (Pacific Ocean 99E. to 70W.) - PROJECTED"
		}, {
			"id": "3395",
			"label": "3395 WGS84/World Mercator (World - between 80S. and 84N.) - PROJECTED"
		}, {
			"id": "3857",
			"label": "3857 WGS84/Web Mercator (World - between 85S. and 85N.) - PROJECTED"
		}, {
			"id": "4171",
			"label": "4171 RGF93 (FR.) - GEOGRAPHIC"
		}, {
			"id": "4230_A",
			"label": "4230 ED50 (EUR.) - GEOGRAPHIC"
		}, {
			"id": "4230_B",
			"label": "4230 ED50 (EUR. - AT. DK. FR. DE. DK. CH.) - GEOGRAPHIC"
		}, {
			"id": "4258",
			"label": "4258 ETRS89 (EUR) - GEOGRAPHIC"
		}, {
			"id": "4265",
			"label": "4265 Monte Mario (IT.) - GEOGRAPHIC"
		}, {
			"id": "4275",
			"label": "4275 NTF - IGN-Fra (FR.) - GEOGRAPHIC"
		}, {
			"id": "4276",
			"label": "4276 NSWC 9Z-2 (World) - GEOGRAPHIC"
		}, {
			"id": "4284_A",
			"label": "4284 Pulkovo 1942 (RU.) [4284_1254] - GEOGRAPHIC"
		}, {
			"id": "4322",
			"label": "4322 WGS 72 (World) - GEOGRAPHIC"
		}, {
			"id": "4324",
			"label": "4324 WGS 72BE (WGS 72) - DMA (World.) - GEOGRAPHIC"
		}, {
			"id": "4326",
			"label": "4326 WGS 1984 - GEOGRAPHIC"
		}, {
			"id": "4807",
			"label": "4807 NTF (Paris) (FR.) - GEOGRAPHIC"
		}, {
			"id": "4901",
			"label": "4901 ATF (Paris) (FR.) - GEOGRAPHIC"
		}],
		"defaultValue": "2154",
		"range": null,
		"format": null,
		"mandatory": "constraint"
	}]
}
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
Using the image properties returned by the search API (step 1), you can use the order API to get the price of the image and order it, using the same POST request:
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
      "properties":       [],
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
         "productTypeId": "'$PRODUCTTYPE_ID'",
         "technicalProduct": {},
         "minimumOrderAreaApplied": 15,
         "descriptionUrl": "http://google.com"
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

If the request above is sent to the endpoint /orders, the image will be produced and delivered to you. You will get a response similar to the following one:
```json
{
	"salesOrderId" : "ORDER_UNIQUE_IDENTIFIER"
}
```
