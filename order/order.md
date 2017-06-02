The ordering API allows you to order archive images and/or task the satellites for new acquisitions, from your own portal. This guide will show you step by step how to order an archive image. 

# Step 1
First of all, you want to look for archive images in our catalogue in order to find the one you will order. You will then use the search API: <https://airbusgeo.github.io/api-docs/search/search.html>. 
The search API will return numerous properties of the images (identifier, footprint...) that you can use in the following steps.

# Step 2
In order to use the ordering API you must have a token. Contact Airbus DS support by sending an email to <contact@geoapi-airbusds.com> to get your token. Once you got a token you can start using the GeoConnector API via the following endpoint: https://order-qual.geoapi-airbusds.com. Please insert the token in the Authorization header field of each request.

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
        aoi: [
          {
            id: 1,
            geometry: {
              type: "Polygon",
              coordinates: [[8.492431640625004, 43.04480541304369], [9.591064453125002, 43.04480541304369], [9.591064453125002, 41.10419094457646], [8.492431640625004, 41.10419094457646],[8.492431640625004, 43.04480541304369]]
            },
            name: "myAccountName"
          }
        ],
        contractId:  "myContractID",
        items: [
          {
            datastripIds: [
              "40582651209160926432R5"
            ],
            aoiId: 1,
            productTypeId: "SPOTArchive10Color",
            properties: [
              {
                key: "uid",
                value: "a4550d93280b4afc70e3949ffec78df1"
              }
            ]
          }
        ],
        primaryMarket: "AGRI",
        secondaryMarket: "AGRI_ENV",  
        customerReference: "myReference",
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
                  value: "myAccount"
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
        voucherCodes:  [
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
            "productTypes" : [
              "amount": 200.0,
              "areaKm2" : 20.0,
              "discountPercentage": 0.0,
              "discountValue": 0.0,
              "price": 200.0,
              "productTypeId": "SPOTArchive10Color",
              "items": [
                "dataStripIds": ["40582651209160926432R5"]
              ]
            ],
            "totalAmountCurrency": 200.0            
}
```

If the request above is sent to the endpoint /orders, the image will be produced and delivered to you. You will get a response similar to the following one:
```json
{
	salesOrderId : ”ORDER_UNIQUE_IDENTIFIER”
}
```



