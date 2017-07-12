The ordering API allows you to order archive images and/or task the satellites for new acquisitions, from your own portal. This guide will show you step by step how to order an archive image. For more information refer to the online reference documentation : <https://airbusgeo.github.io/geoapi-viewer/?url=https://airbusgeo.github.io/api-docs/order.yaml>

# Step 1
First of all, you want to look for archive images in our catalogue in order to find the one you will order. You will then use the search API: <https://airbusgeo.github.io/api-docs/search/search.html>. 
The search API will return numerous properties of the images (identifier, footprint...) that you can use in the following steps.

# Step 2
In order to use the ordering API you must have a token. Send a POST request to : <https://is-qual.geoapi-airbusds.com:9443/ADS-ID/core/connect/token>. The body of the request must contain:
```
client_id=xxxxxx&client_secret=xxxxxxxx&scope=order.api&grant_type=client_credentials
```
where xxxxxx must be replaced by the values sent to you in a crypted file.

Once you got a token you can start using the GeoConnector API via the following endpoint: <https://order-qual.geoapi-airbusds.com:8443>. Insert the token in the Authorization field of each request you send to the Order API. Note that the header of the requests must also contain the field X-ADS-Username.

Requests header:
- Field name : Authorization (value = token)
- Field name : X-ADS-Username (value in the crypted file you received by email)

# Step 3
This step is optional. You might want to get the remaining credit on your contract. In this case, you send a GET /me/contracts to the server and get a response similar to:
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

# Step 4
Using the image properties returned by the search API (step 1), you can use the order API to get the price of the image and order it, using the same POST request:
```json
{
        "aoi": [
          {
            "id": 1,
            "geometry": {
              "type": "Polygon",
              "coordinates": [[8.492431640625004, 43.04480541304369], [9.591064453125002, 43.04480541304369], [9.591064453125002, 41.10419094457646], [8.492431640625004, 41.10419094457646],[8.492431640625004, 43.04480541304369]]
            },
            "name": "myAccountName"
          }
        ],
        "contractId":  "myContractID",
        "items": [
          {
            "datastripIds": [
              "40582651209160926432R5"
            ],
            "aoiId": 1,
            "productTypeId": "SPOTArchive10Color",
            "properties": [
              {
                "key": "uid",
                "value": "a4550d93280b4afc70e3949ffec78df1"
              }
            ]
          }
        ],
        "primaryMarket": "AGRI",
        "secondaryMarket": "AGRI_ENV",  
        "customerReference": "myReference",
        "options": [
          {
            "key": "deliveryType",
            "value": "network"
          }
        ],
        "optionsPerProductType": [
          {
            "productTypeId": "SPOTArchive10Color",
            "licence": "eula_5",
            "options": [
                {
                  "key": "image_format",
                  "value": "dimap_geotiff"
                },
                {
                  "key": "Account",
                  "value": "myAccount"
                },
                {
                  "key": "projection_1",
                  "value": "32632"
                },
                {
                  "key": "processing_level",
                  "value": "level_3"
                }
            ]
          }
        ],
        "voucherCodes":  [
        ]
      }
```

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
The ordering API allows you to order archive images and/or task the satellites for new acquisitions, from your own portal. This guide will show you step by step how to order an archive image. For more information refer to the online reference documentation : <https://airbusgeo.github.io/geoapi-viewer/?url=https://airbusgeo.github.io/api-docs/order.yaml>

# Step 1
First of all, you want to look for archive images in our catalogue in order to find the one you will order. You will then use the search API: <https://airbusgeo.github.io/api-docs/search/search.html>. 
The search API will return numerous properties of the images (identifier, footprint...) that you can use in the following steps.

# Step 2
In order to use the ordering API you must have a token. Send a POST request to : <https://is-qual.geoapi-airbusds.com:9443/ADS-ID/core/connect/token>. The body of the request must contain:
```
curl -X POST -H "Content-Type: application/x-www-urlencoded" -d '{ 
client_id=xxxxxx
client_secret=xxxxxxxx
scope=order.api
grant_type=client_credentials
}' "https://search.geoapi-airbusds.com/api/v1/search" 

```
where xxxxxx must be replaced by the values sent to you in a crypted file.

Once you got a token you can start using the GeoConnector API via the following endpoint: <https://order-qual.geoapi-airbusds.com:8443>. Insert the token in the Authorization field of each request you send to the Order API. Note that the header of the requests must also contain the field X-ADS-Username.
Requests header:

```shell
export MY_TOKEN=***INSERT YOUR API KEY HERE***
MY-USERNAME=***INSERT THE VALUE IN THE CRYPTED FILE YOU RECEIVED BY EMAIL HERE***
```

- Field name : Authorization (value = token)
- Field name : X-ADS-Username (value in the crypted file you received by email)

# Step 3
This step is optional. You might want to get the remaining credit on your contract. In this case, you send a GET /me/contracts to the server:
```
curl -X GET -H "Authorization: $MY_TOKEN –H “X-ADS-Username: $MY_USERNAME” 
"https://order-qual.geoapi-airbusds.com:8443/api/v1/ordering/me/contracts"
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

# Step 4
This step is mandatory. You might want to get the remaining credit on your contract. In this case, you send a GET /me/contracts to the server:
```
curl -X POST -H "Authorization: $MY_TOKEN –H “X-ADS-Username: $MY_USERNAME” 
-d '{
  {
      "aoi": [ {
          "polygonId": 1,
          "geometry": "{
              "type": 'Polygon',
              "coordinates": [[8.492431640625004, 43.04480541304369],[9.591064453125002, 43.04480541304369],
                [9.591064453125002, 41.10419094457646], [8.492431640625004, 41.10419094457646],[8.492431640625004, 43.04480541304369]]
            }"
        }
      ]
    }'
"https://order-qual.geoapi-airbusds.com:8443/api/v1/ordering/productTypes/{yourProductTypeId}/options"
```

Note that you should replace the coordinates by the ones you chose to search your picture in the Search API.

You would get a response similar to:
```json
[
	availableOptions: {
	  category: "technical_product",
	  defaultValue: "ortho",
	  label: "PROCESSING_LEVEL",
	  mandatory: "true",
	  name: "processing_level",
	  type: "list",
	  values: [
		{
		  id: "ortho",
		  label: "Ortho"
		},
		{
		  id: "projected",
		  label: "Projected"
		},
		{
		  id: "primary",
		  label: "Primary"
		}
	  ]
	}
]  
```

# Step 5
Using the image properties returned by the search API (step 1), you can use the order API to get the price of the image and order it, using the same POST request:
```
curl -X POST -H "Authorization: $MY_TOKEN –H “X-ADS-Username: $MY_USERNAME” 
-d '{
        aoi: [
          {
            id: 1,
            geometry: "{
              type: 'Polygon',
              coordinates: [[8.492431640625004, 43.04480541304369],[9.591064453125002, 43.04480541304369],
                [9.591064453125002, 41.10419094457646], [8.492431640625004, 41.10419094457646],[8.492431640625004, 43.04480541304369]]
            }",
            name: "test_for_partners"
          }
        ],
        contractId:  $CONTRACT_ID,
        items: [
          {
            datastripIds: [
              "40582651209160926432R5"
            ],
            aoiId: 1,
            productTypeId: "SPOTArchive10Color",
            properties: []
          }
        ],
        primaryMarket: "AGRI",
        secondaryMarket: "AGRI_ENV",  
        customerReference: "The order reference that will appear in invoicing",
        options: [
          {
            key: "deliveryType",
            value: "network"
          }
       ],
        optionsPerProductType: [
          {
            productTypeId: "SPOTArchive10Color",
            licence: "eula_5",
            options: [
                {
                  key: "image_format",
                  value: "dimap_geotiff"
                },
                {
                  key: "Account",
                  value: "DDPORTAL"
                },
                {
                  key: "projection_1",
                  value: "32632"
                },
                {
                  key: "processing_level",
                  value: "level_3"
                }
            ]
          }
        ],
        voucherCodes:  []
      }'
"https://order-qual.geoapi-airbusds.com:8443/api/v1/ordering/order"
```

Note that you should replace the coordinates by the ones you want, the productTypeId by the one of the product type you want and the datastripId by the searchAPI's resquest sourceId. The optionsPerProductType must be filled by
all the mandatory fields from the productTypes options. For instance, in the given productTypes options example, you should select the name (processing_level in that cas) as key and the wanted value id ("ortho" for instance) as value.


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
