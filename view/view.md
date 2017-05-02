# View API documentation

## Open API

The Open API specification can be [downloaded](../view.yaml) or [consulted](http://airbusgeo.github.io/geoapi-viewer/?url=https://airbusgeo.github.io/api-docs/view.yaml) directly in Airbus API Viewer.

## Prerequisites

In order to use the View API you need first to obtain an API key by requesting it to the customer care support or the [Contact Us](http://www.intelligence-airbusds.com/contact/) page.

## Usage examples

### Access to layers

This example demonstrate how to access the view layers for displaying them in a web or mobile application.

First, define your API key so that the example commands can use it:

```shell
export MY_API_KEY=***INSERT YOUR API KEY HERE***
```

In order to access the View API layers, you must first retrieve the layers identifiers. The ```/me``` endpoint list all the layers you have access to: 

```shell
curl -X GET -H "Authorization: $MY_API_KEY" "https://view.geoapi-airbusds.com/api/v1/me"
```

The response should look like:

```json
{
  "canCompose": true,
  "email": "john.doe@airbus.com",
  "externalId": "7743610706",
  "firstname": "John",
  "id": "4f586b1f-9999-4ba6-8188-8afdf2684126",
  "isSubscriptionSuspended": false,
  "lastname": "Doe",
  "maps": {
    "compositions": [],
    "imagery": {
      "id": "CF69E122-9999-11E6-B6B4-3F1489348498",
      "layers": [
        {
          "id": "53744984-9999-11E7-80B3-420189964461",
          "offerName": "OneLive"
        }
      ],
      "links": {
        "ags": "https://view.geoapi-airbusds.com/api/v1/map/imagery.ags/services",
        "agsRestDirect": "https://view.geoapi-airbusds.com/mugg/rest/services/CF69E122-9999-11E6-B6B4-3F1489348498/MapServer",
        "agsSoapDirect": "https://view.geoapi-airbusds.com/mugg/soap/CF69E122-9999-11E6-B6B4-3F1489348498/services",
        "kml": "https://view.geoapi-airbusds.com/api/v1/map/imagery.kml",
        "kmlDirect": "https://view.geoapi-airbusds.com/mugg/kml/CF69E122-9999-11E6-B6B4-3F1489348498",
        "wms": "https://view.geoapi-airbusds.com/api/v1/map/imagery.wms",
        "wmsDirect": "https://view.geoapi-airbusds.com/mugg/wms/CF69E122-9999-11E6-B6B4-3F1489348498?",
        "wmts": "https://view.geoapi-airbusds.com/api/v1/map/imagery.wmts",
        "wmtsDirect": "https://view.geoapi-airbusds.com/mugg/wmts/CF69E122-9999-11E6-B6B4-3F1489348498?"
      }
    },
    "metadata": {
      "id": "CF11E595-9999-11E6-B6B3-0F918242EB09",
      "layers": [
        {
          "id": "59945568-9999-11E7-80B3-424854475111",
          "offerName": "OneLive-Metadata"
        }
      ],
      "links": {
        "ags": "https://view.geoapi-airbusds.com/api/v1/map/imagery-composition-detail.ags/services",
        "agsRestDirect": "https://view.geoapi-airbusds.com/mugg/rest/services/CF11E595-9999-11E6-B6B3-0F918242EB09/MapServer",
        "agsSoapDirect": "https://view.geoapi-airbusds.com/mugg/soap/CF11E595-9999-11E6-B6B3-0F918242EB09/services",
        "kml": "https://view.geoapi-airbusds.com/api/v1/map/imagery-composition-detail.kml",
        "kmlDirect": "https://view.geoapi-airbusds.com/mugg/kml/CF11E595-9999-11E6-B6B3-0F918242EB09",
        "wfs": "https://view.geoapi-airbusds.com/api/v1/map/imagery-composition-detail.wfs",
        "wfsDirect": "https://view.geoapi-airbusds.com/mugg/wfs/885964C9-9999-4DB5-815D-48A01941BF0D?",
        "wms": "https://view.geoapi-airbusds.com/api/v1/map/imagery-composition-detail.wms",
        "wmsDirect": "https://view.geoapi-airbusds.com/mugg/wms/CF11E595-9999-11E6-B6B3-0F918242EB09?",
        "wmts": "https://view.geoapi-airbusds.com/api/v1/map/imagery-composition-detail.wmts",
        "wmtsDirect": "https://view.geoapi-airbusds.com/mugg/wmts/CF11E595-9999-11E6-B6B3-0F918242EB09?"
      }
    }
  },
  "subscriptionAdministrator": [],
  "subscriptions": [
    "119934e0-9999-442c-8a14-fae18754458d"
  ],
  "systemRoles": [],
  "userPreference": {}
}
```

The available layers can be found under the ```/maps/imagery/layers``` path. Depending on the offers you have subscribed to, you can see one to many layers. In our Example, the user has subscribed to the ```OneLive``` layer with the identifier ```53744984-9999-11E7-80B3-420189964461```. This identifier is user specific and must be retrieved from the previous request.

```json
{
  ...
  "maps": {
    ...
    "imagery": {
      "id": "...",
      "layers": [
        {
          "id": "53744984-9999-11E7-80B3-420189964461",
          "offerName": "OneLive"
        }
      ]
  ...
}
```


Set your own identifier using the command below. 
```shell
export MY_LAYER_ID=***INSERT YOUR LAYER IDENTIFIER HERE***
```

Following the WMTS specification, you can then request the tiles you need by converting your coordinates in tile column and row :

```shell
curl -X GET -H "Authorization: $MY_API_KEY" "https://view.geoapi-airbusds.com/api/v1/map/imagery.wmts?layer=$MY_LAYER_ID&tilematrixset=4326&Service=WMTS&Request=GetTile&Version=1.0.0&Format=image/png&TileMatrix=17&TileCol=132122&TileRow=33783"

curl -X GET -H "Authorization: $MY_API_KEY" "https://view.geoapi-airbusds.com/api/v1/map/imagery.wmts?layer=$MY_LAYER_ID&tilematrixset=4326&Service=WMTS&Request=GetTile&Version=1.0.0&Format=image/png&TileMatrix=17&TileCol=132123&TileRow=33783"

curl -X GET -H "Authorization: $MY_API_KEY" "https://view.geoapi-airbusds.com/api/v1/map/imagery.wmts?layer=$MY_LAYER_ID&tilematrixset=4326&Service=WMTS&Request=GetTile&Version=1.0.0&Format=image/png&TileMatrix=17&TileCol=132122&TileRow=33784"

curl -X GET -H "Authorization: $MY_API_KEY" "https://view.geoapi-airbusds.com/api/v1/map/imagery.wmts?layer=$MY_LAYER_ID&tilematrixset=4326&Service=WMTS&Request=GetTile&Version=1.0.0&Format=image/png&TileMatrix=17&TileCol=132123&TileRow=33784"
```

The 4 requested tiles should display Toulouse Capitol:

![capitol](images/capitol.png "Tiles of toulouse Capitol")







