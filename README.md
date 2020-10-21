
# User Movement Analytics REST API

## Authentication

### Authentication Request

```shell script
curl \
  -XPOST \
  -d 'username=<username>&password=<password>&client_id=gometro-uma-service&grant_type=password' \
  https://identity.gometroapp.com/auth/realms/platform/protocol/openid-connect/token
```

### Authentication Response

```json
{
  "access_token": "<access_token>",
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": "<refresh_token>",
  "token_type": "bearer",
  "not-before-policy": 0,
  "session_state": "<session_state>",
  "scope": "email profile"
}
```

### Authenticated Request

```shell script
curl \
  -XGET \
  -H 'Authorization: Bearer <access_token>' \
  https://api.gometroapp.com/uma/v1/participants/{participantId}/trips?fromDate=2020-10-01&toDate=2020-10-03
```

## Participant Identifier

To ensure that no Personally Identifiable Information can be unintentionally leaked through access logs, 
the participant's identifier should be converted to lower-case and hashed using the SHA-256 algorithm. 

### Shell
```shell script
$> echo -n geoff.vader@example.com | shasum -a 256
335329fafadd47f63695c2256d6d4a56c652c9885018856b81cc612950b0298b  -
```  

## Endpoints

Base URL: https://api.gometroapp.com/uma/v1

***

### Find Trips
GET    /participants/{participantId}/trips

#### Request Body
None

#### Parameters
| Name     | Data Type | Description                                                                         | Example    |
|----------|-----------|-------------------------------------------------------------------------------------|------------|
| fromDate | date      | Return all trips with an origin departure time greater than or equal to this date   | 2020-10-01 |
| toDate   | date      | Return all trips with a destination arrival time greater than or equal to this date | 2020-10-02 |

#### Response Body
Array of Trip

***

### Update Leg 
PUT    /participants/{participantId}/trips/{tripId}/legs/{legId}

#### Request Body
```json
{
  "fromStopoverId": "74984965-6747-44e1-8b26-71cb34e7fe3b",
  "toStopoverId": "35e31f9d-85eb-4c26-9bc1-1685f5175648",
  "mode": "BUS",
  "polyline": "rhhoCnp_|GFGGGAAGIEICKCGACCGEGEGIKSQk@i@[Y_A{@IIm@k@SS}AwA_@]GGc@a@[YYWMKQM{@q@QOIG][c@_@m@g@MMq@k@[[}@y@OMs@o@e@c@MOMMOUKQIKIKIKMMQSe@c@w@u@}AwAUQYYIIOMi@g@k@i@WWYU?Ao@k@[[m@k@mAiA{@y@[Y}@y@s@m@q@o@oD_D[W_@_@AAOOQQYYYYGEQQGEIGEECGc@_@oAeAk@e@OMMM[WYY}@_Aa@c@g@c@u@q@cAaAaB{Ai@g@_@]IJ"
}
```

#### Parameters
None

***

### Split Leg 
POST   /participants/{participantId}/trips/{tripId}/legs/{legId}/split

#### Request Body
```json
{
  "place": {
    "latitude": -23.6254326,
    "longitude": -46.6827622,
    "address": "Morumbi B/C 1",
    "purpose": "TRANSFER"
  },
  "arrivalTime": "2020-10-08T11:24:49+0000",
  "departureTime": "2020-10-08T11:24:49+0000"
}
```

or 

```json
{
  "placeId": "6f0738ee-52b5-4e02-b9d1-54813e101807",
  "arrivalTime": "2020-10-08T11:24:49+0000",
  "departureTime": "2020-10-08T11:24:49+0000"
}
```

#### Parameters
None

***

### Create Stopover 
POST   /participants/{participantId}/stopovers

#### Request Body
```json
{
  "place": {
    "latitude": -23.6421685,
    "longitude": -46.6938902,
    "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
    "purpose": "HOME"
  },
  "arrivalTime": "2020-10-07T23:36:52+0000",
  "departureTime": "2020-10-08T11:02:12+0000"
}
```

or

```json
{
  "placeId": "80dd916a-6560-4912-9c77-b5fd9d56898f",
  "arrivalTime": "2020-10-07T23:36:52+0000",
  "departureTime": "2020-10-08T11:02:12+0000"
}
```

#### Parameters
None

***

### Update Stopover 
PUT    /participants/{participantId}/stopovers/{stopoverId}

#### Request Body
```json
{
  "place": {
    "latitude": -23.6421685,
    "longitude": -46.6938902,
    "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
    "purpose": "HOME"
  },
  "arrivalTime": "2020-10-07T23:36:52+0000",
  "departureTime": "2020-10-08T11:02:12+0000"
}
```

or

```json
{
  "placeId": "80dd916a-6560-4912-9c77-b5fd9d56898f",
  "arrivalTime": "2020-10-07T23:36:52+0000",
  "departureTime": "2020-10-08T11:02:12+0000"
}
```

#### Parameters
None

***

### Remove Stopover 
DELETE /participants/{participantId}/stopovers/{stopoverId}

#### Request Body
None

#### Parameters
None

***

### Find Places
GET    /participants/{participantId}/places

#### Request Body
None

#### Parameters
None

***

### Create Place
POST   /participants/{participantId}/places

#### Request Body
```json
{
  "latitude": -23.6421685,
  "longitude": -46.6938902,
  "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
  "purpose": "HOME"
}
```

#### Parameters
None

***

### Update Place
PUT    /participants/{participantId}/places/{placeId}

#### Request Body
```json
{
  "latitude": -23.6421685,
  "longitude": -46.6938902,
  "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
  "purpose": "HOME"
}
```

#### Parameters
None

***

### Remove Place
DELETE /participants/{participantId}/places/{placeId}

#### Request Body
None

#### Parameters
None

***

# Model

## Leg

### Properties

| Name         | Data Type | Description                                              |
|--------------|-----------|----------------------------------------------------------|
| id           | string    | ID for the Leg                                           |
| fromStopover | Stopover  | The Stopover from which the Leg started                  | 
| toStopover   | Stopover  | The Stopover at which the Leg ended                      | 
| mode         | Mode      | The Mode of Transport that was used to complete the Leg. | 
| polyline     | string    | The encoded poyline that describes the Leg.              |

### Example

```json
{
  "id": "0db3e059-495d-4bc6-9111-0c6c56dd9050",
  "fromStopover": {
    "id": "74984965-6747-44e1-8b26-71cb34e7fe3b",
    "place": {
      "id": "43615d6b-9034-4e6c-8096-056cb4fc7a6c",
      "latitude": -23.6405872,
      "longitude": -46.6971948,
      "address": "Marechal Deodoro B/C",
      "purpose": "TRANSFER"
    },
    "arrivalTime": "2020-10-08T11:11:57+0000",
    "departureTime": "2020-10-08T11:14:28+0000"
  },
  "toStopover": {
    "id": "35e31f9d-85eb-4c26-9bc1-1685f5175648",
    "place": {
      "id": "6f0738ee-52b5-4e02-b9d1-54813e101807",
      "latitude": -23.6254326,
      "longitude": -46.6827622,
      "address": "Morumbi B/C 1",
      "purpose": "TRANSFER"
    },
    "arrivalTime": "2020-10-08T11:24:49+0000",
    "departureTime": "2020-10-08T11:24:49+0000"
  },
  "mode": "BUS",
  "polyline": "rhhoCnp_|GFGGGAAGIEICKCGACCGEGEGIKSQk@i@[Y_A{@IIm@k@SS}AwA_@]GGc@a@[YYWMKQM{@q@QOIG][c@_@m@g@MMq@k@[[}@y@OMs@o@e@c@MOMMOUKQIKIKIKMMQSe@c@w@u@}AwAUQYYIIOMi@g@k@i@WWYU?Ao@k@[[m@k@mAiA{@y@[Y}@y@s@m@q@o@oD_D[W_@_@AAOOQQYYYYGEQQGEIGEECGc@_@oAeAk@e@OMMM[WYY}@_Aa@c@g@c@u@q@cAaAaB{Ai@g@_@]IJ"
}
```

## Mode 

### Values

| Name  | Description    |
|-------|----------------|
| BUS   | Riding a bus   |
| DRIVE | Driving a car  | 
| TRAIN | Riding a train | 
| WALK  | Walking        |


## Place

### Properties

| Name      | Data Type | Description                                             |
|-----------|-----------|---------------------------------------------------------|
| id        | string    | ID for the Place                                        |
| latitude  | number    | The latitude on which the place is located              | 
| longitude | number    | The longitude on which the place is located             | 
| address   | string    | The address at which the place is located               | 
| purpose   | Purpose   | The purpose of the place, ie. Home, Work, Shopping etc. |

### Example

```json
{
  "id": "80dd916a-6560-4912-9c77-b5fd9d56898f",
  "latitude": -23.6421685,
  "longitude": -46.6938902,
  "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
  "purpose": "HOME"
}
```

## Purpose

### Values

| Name     | Description                                                                      |
|----------|----------------------------------------------------------------------------------|
| DINING   | A Place at which the participant dines, ie. eats or drinks                       |
| HOME     | The Place at which the participant resides and/or sleeps                         | 
| SHOPPING | The Place at which the participant purchases groceries                           | 
| TRANSFER | A Place at which the participant transfers from one mode of transport to another | 
| WORK     | The address at which the place is located                                        |

## Stopover

### Properties

| Name          | Data Type | Description                                                         |
|---------------|-----------|---------------------------------------------------------------------|
| id            | string    | ID for the Stopover                                                 |
| place         | Place     | The Place at which the participant stopped                          | 
| arrivalTime   | datetime  | The data and time at which the participant arrived at this Place    | 
| departureTime | datetime  | The data and time at which the participant departed from this Place |

### Example

```json
{
  "id": "cf2ca358-ae0a-4e1c-b986-f80ca5cddc70",
  "place": {
    "id": "80dd916a-6560-4912-9c77-b5fd9d56898f",
    "latitude": -23.6421685,
    "longitude": -46.6938902,
    "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
    "purpose": "HOME"
  },
  "arrivalTime": "2020-10-07T23:36:52+0000",
  "departureTime": "2020-10-08T11:02:12+0000"
}
```

## Trip

### Properties

| Name        | Data Type    | Description                                                                 |
|-------------|--------------|-----------------------------------------------------------------------------|
| id          | string       | ID for Trip                                                                 |
| legs        | array of Leg | One or more Legs that were completed between the Origin and the Destination |

### Example 

```json
{
  "id": "05416c74-2511-44d9-bb56-e34199b59514",
  "legs": [
    {
      "id": "76257e2c-a95d-4030-becd-6da275d65c44",
      "from": {
        "id": "cf2ca358-ae0a-4e1c-b986-f80ca5cddc70",
        "place": {
          "id": "80dd916a-6560-4912-9c77-b5fd9d56898f",
          "latitude": -23.6421685,
          "longitude": -46.6938902,
          "address": "R. Min. Roberto Cardoso Alves, 1371 - Santo Amaro, São Paulo - SP, 04737-001, Brazil",
          "purpose": "HOME"
        },
        "arrivalTime": "2020-10-07T23:36:52+0000",
        "departureTime": "2020-10-08T11:02:12+0000"
      },
      "to": {
        "id": "74984965-6747-44e1-8b26-71cb34e7fe3b",
        "place": {
          "id": "43615d6b-9034-4e6c-8096-056cb4fc7a6c",
          "latitude": -23.6405872,
          "longitude": -46.6971948,
          "address": "Marechal Deodoro B/C",
          "purpose": "TRANSFER"
        },
        "arrivalTime": "2020-10-08T11:11:57+0000",
        "departureTime": "2020-10-08T11:14:28+0000"
      },
      "mode": "WALK",
      "polyline": "prhoCx{~{GANw@|Ay@`B?@?@@??@?@@@?@?@?@?@A?GJ?@A?A?A?A?A?sBbE?@?@@??@?@?@?@GLA?A??@A?A?A?A?a@x@GL_@t@BD@BBFBHDHFHHHPPRRbAbA\\\\\\Z\\\\IN_@_@][]]cAcASSSQ?A"
    },
    {
      "id": "0db3e059-495d-4bc6-9111-0c6c56dd9050",
      "from": {
        "id": "74984965-6747-44e1-8b26-71cb34e7fe3b",
        "place": {
          "id": "43615d6b-9034-4e6c-8096-056cb4fc7a6c",
          "latitude": -23.6405872,
          "longitude": -46.6971948,
          "address": "Marechal Deodoro B/C",
          "purpose": "TRANSFER"
        },
        "arrivalTime": "2020-10-08T11:11:57+0000",
        "departureTime": "2020-10-08T11:14:28+0000"
      },
      "to": {
        "id": "35e31f9d-85eb-4c26-9bc1-1685f5175648",
        "place": {
          "id": "6f0738ee-52b5-4e02-b9d1-54813e101807",
          "latitude": -23.6254326,
          "longitude": -46.6827622,
          "address": "Morumbi B/C 1",
          "purpose": "TRANSFER"
        },
        "arrivalTime": "2020-10-08T11:24:49+0000",
        "departureTime": "2020-10-08T11:24:49+0000"
      },
      "mode": "BUS",
      "polyline": "rhhoCnp_|GFGGGAAGIEICKCGACCGEGEGIKSQk@i@[Y_A{@IIm@k@SS}AwA_@]GGc@a@[YYWMKQM{@q@QOIG][c@_@m@g@MMq@k@[[}@y@OMs@o@e@c@MOMMOUKQIKIKIKMMQSe@c@w@u@}AwAUQYYIIOMi@g@k@i@WWYU?Ao@k@[[m@k@mAiA{@y@[Y}@y@s@m@q@o@oD_D[W_@_@AAOOQQYYYYGEQQGEIGEECGc@_@oAeAk@e@OMMM[WYY}@_Aa@c@g@c@u@q@cAaAaB{Ai@g@_@]IJ"
    },
    {
      "id": "12af7360-8433-44b0-a4ae-60f7c1efb13f",
      "from": {
        "id": "35e31f9d-85eb-4c26-9bc1-1685f5175648",
        "place": {
          "id": "6f0738ee-52b5-4e02-b9d1-54813e101807",
          "latitude": -23.6254326,
          "longitude": -46.6827622,
          "address": "Morumbi B/C 1",
          "purpose": "TRANSFER"
        },
        "arrivalTime": "2020-10-08T11:24:49+0000",
        "departureTime": "2020-10-08T11:24:49+0000"
      },
      "to": {
        "id": "4c9d00ed-37be-408e-a72b-6743aef0bbe9",
        "place": {
          "id": "3fdb03be-11e3-48c6-bae0-a9852f3d7950",
          "latitude": -23.6239044,
          "longitude": -46.6885951,
          "address": "Rua Jataituba, 29 - Cidade Monções, São Paulo - SP, 04704-090, Brazil",
          "purpose": "DINING"
        },
        "arrivalTime": "2020-10-08T11:35:29+0000",
        "departureTime": "2020-10-08T12:40:05+0000"
      },
      "mode": "BUS",
      "polyline": "xieoCnv|{GOOEEk@k@MNMPi@r@gAzAIJY`@[b@OPY`@SXMPIJ]b@]d@g@l@KLCDKNAFCHENCVANGd@Gd@ADIt@Gf@CNM~@CTIp@E^EXENdAl@f@Z@@PLZRLJPHZRLFDDn@XLFKX"
    }
  ]
}
```  
