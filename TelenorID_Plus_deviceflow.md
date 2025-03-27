## TelenorID\+ Device Authorization Endpoint

## /deviceauthorization

The ```/connect/deviceauthorization``` API is used to to start a end-user login from a device without good text input. Eg. T-we setupboxes, smart-TV's.
This endpoint implements the [RFC 8628 standard](https://datatracker.ietf.org/doc/html/rfc8628).

## Prerequists

This end-point sets the following prerequists to the device and the end-user:

   - The device is already connected to the Internet.
   - The device is able to make outbound HTTPS requests.
   - The device is able to display or otherwise communicate a URI and code sequence to the user.
   - The user has a secondary device (e.g., personal computer or smartphone) from which they can process the request.

## Flow


      +----------+                                +----------------+
      |          |>---(A)-- Client Identifier --->|                |
      |          |                                |                |
      |          |<---(B)-- Device Code,      ---<|                |
      |          |          User Code,            |                |
      |  Device  |          & Verification URI    |                |
      |  Client  |                                |                |
      |          |  [polling]                     |                |
      |          |>---(E)-- Device Code       --->|                |
      |          |          & Client Identifier   |                |
      |          |                                |  Authorization |
      |          |<---(F)-- Access Token      ---<|     Server     |
      +----------+   (& Optional Refresh Token)   |                |
            v                                     |                |
            :                                     |                |
           (C) User Code & Verification URI       |                |
            :                                     |                |
            v                                     |                |
      +----------+                                |                |
      | End User |                                |                |
      |    at    |<---(D)-- End user reviews  --->|                |
      |  Browser |          authorization request |                |
      +----------+                                +----------------+

See the [RFC 8628 standard](https://datatracker.ietf.org/doc/html/rfc8628) for a more detailed description of the flow chart.

## Input

The endpoint supports HTTP POST.
The body should have content-type ```x-www-form-urlencoded``` or ```application/json``` with the following keys and values.
The credentials could also be omitted from the body and submitted in the Authorization header with Basic Auth.

| Parameter | Description  | Required |
|-------------|-------------|-------------|
| ```client_id``` | identifier of the client | True |
| ```client_secret```	| client secret either in the post body, or as a basic authentication header.  | False |
| ```scope```	| one or more registered scopes, see more info [here](TelenorID_Plus_-_scopes.md) | True |

## Response


| Parameter | Description                                                                                                                                      | Required |
| ----------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| ``` device_code``` | identifier of the user                                                                                                                           | True |
| ```user_code```	| A 9 digit verification code                                                                                                                      | False |
| ```verification_uri```	| The end-user verification URI on the authorization server                                                                                        |  True |
| ```verification_uri_complete```	| ```verification_uri``` + ```user_code```, can be used to generate a QR-code that can be scanned by the end-user                                  |  True |
| ``` expires_in```	| The lifetime in seconds of the ```device_code``` and ```user_code```. Default value: ```1800``` - 30 minutes.                                    |  True |
| ```interval```	| The minimum amount of time in seconds that the client SHOULD wait between polling requests to the token endpoint. Default value: ```5``` seconds |  True |

The user must then be directed to the verification page. Either ```verification_uri``` or ```verification_uri_complete``` must be used for this.
There the user will input the ```user_code``` and approve the request by logging into their TelenorID account.
This must be completed before the ```expires_in``` lifetime has been exceeded.
The ```interval``` provided is how often the client can poll for the token from the token endpoint: ```/connect/token```.

## Polling for token
When the client is polling for the token it has to first wait for the specified time in the ```interval``` to pass or it will receive a slow down response.
It also has to submit a POST request to ```/connect/token``` with a body of content-type ```x-www-form-urlencoded``` or ```application/json``` and the following keys and values.

| Parameter           | Description                                                                                                      | Required           |
|---------------------|------------------------------------------------------------------------------------------------------------------|--------------------|
| ```grant_type```    | Will always be ```urn:ietf:params:oauth:grant-type:device_code``` for device-flow                                | true               |
| ```device_code```   | The ```device_code``` from the response                                                                          | true               |
| ```client_id```     | Identifier of the client. Can also be sent as basic auth                                                         | true               |
| ```client_secret``` | Corresponding client secret. This is only applicable for clients with a secret. Can also be sent with basic auth | true if applicable |
| ```device_code```   | Identifier of the user - received from the auth endpoint earlier.                                                | true               |

Submitting these values to the ```/connect/token``` endpoint will result in one of three responses.
The success response is as follows:
```
{
    "id_token": "<id-token-jwt>",
    "access_token": "<access-token-jwt>",
    "expires_in": 3600,
    "token_type": "Bearer",
    "refresh_token": "<refresh-token>",
    "scope": "email offline_access openid profile"
}

```
### Unsuccessful responses:
This means that the request is invalid. Either the authorization request related to the ```device_code``` has been completed and therefore deleted, it has been expired and cleaned up or it never existed in the first place.
```
{
    "error": "invalid_grant"
}
```

This means the user hasn't approved the authorization request yet on the activation page.
```
{
    "error": "authorization_pending"
}
```