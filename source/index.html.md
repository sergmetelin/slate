---
title: Progimage API Reference

language_tabs:
  - shell: cURL

search: true
---

# Introduction

    All API access is performed over HTTPS and accessed from the progimage.ngrok.io domain.

# Errors
    
    If an error occurs, the API will respond with the appropriate HTTP status code and an error message object which contains an error code and a humanly readable message.
    
    List of error codes
    
    Error Code | HTTP Status Code | Meaning
    -------------- | -------------- | --------------
    bad_request | 400 Bad Request | The request was malformed
    bad_credentials | 401 Unathorized | Invalid credentials were used to authenticate the request.
    not_found | 404 Not Found | The requested resource does not exist or is not publicly available.
    invalid_request | 422 Unprocessable Entity | The resource has missing parameters
    rate_limit_exceeded | 429 Too Many Requests | API request rate limit exceeded. See rate limiting.

# HTTP Request Methods
    
    Where possible, the Progimage REST API strives to use appropriate HTTP methods for each action:
    
    Method | Action
    -------------- | --------------
    GET | Retrieve one or more resources.
    POST | Create a resource or perform a custom action.

# Pagination

    Endpoints rely on traditional offset and limit query parameters. If query params are not present, default values are applied. Default offset value is 0 and default limit value is 10. 
    
# Rate Limiting
    
    Progimage API employs strict rate limiting depending on the authentication method. For OAuth 2.0 authenticated requests, you can make up to 20 requests per 5 seconds for each authorized user.
    
    Hitting the wall
    
    If you exceed the request rate limit, the Progimage API will return a 429 Too Many Requests
    
# Cross Origin Resource Sharing (CORS)

    The Progimage API supports Cross Origin Resource Sharing (CORS) for requests, independent of the request origin.

# Authentication

    The Progimage API uses OAuth 2.0 for authentication. In order to use OAuth 2.0, you need to register an API application, which will be assigned a unique client ID and client secret.

## 1. Once you have registered your application, you need to authorize users to be able to access the Progimage API as the specific user.
      
    To begin the authorization flow, redirect the user to the following URL on Progimage:
  
    `GET https://progimage.ngrok.io/oauth/authorize`
    
    Parameters
    
    Name | Type | Description
    -------------- | -------------- | --------------
    client_id | string | Your app client ID
    response_type | string | Value should be 'code'
    redirect_url | string | URL to redirect to

## 2. Progimage redirects back to your site
    
    If the user chooses to authorize your application, Progimage redirects back to your site at the callback URL you configured for your application when registering it. The URL will include with an intermediate code in the code request parameter. If no code parameter is provided, it means that the user did not choose to authorize your application.
    
## 3. Exchange the intermediate code for an access token
       
    Once you have the intermediate code, it must be exchanged for an access token that will be used for all authenticated requests. Access tokens are obtained at the following URL:
    
    `POST https://api.progimage.ngrok.io/oauth/token`

    Parameters
        
    Name | Type | Description
    -------------- | -------------- | --------------
    client_id | string | Your app client ID
    client_secret | string | Your app client secret
    code | string | The intermediate code received at step 2
    
## 4. Response
    
    Upon successful exchange of the intermediate code, Progimage returns a JSON response with the access token and token type issued. Access tokens are guaranteed to be alphanumerical strings of 64 byte lengths:
    
```json
{
  "access_token": "1395522a376c577b7a3193862dca1623f98605d5af756b3bd55f3857cd31a95f",
  "token_type": "bearer",
  "expires_in": 86400,
  "scope": "api",
  "created_at": 1480825975
}
```

## 5. Hit up some endpoints

    Now, by supplying the access token in the Authorization header of your requests, using the “Bearer” token type, you can get access to the endpoints:
    
    `Authorization: Bearer mUZrfE9MJySA1YTyGl9HsMjIypWNtl9jqjpfqxMKakdV00Fkh6C38gZkztrtCzWS`

# Scopes

    Not covered in current version
    
# Get details about the authenticated user

    This endpoint exposes details about the authenticated user.
     
> `GET /user`

```json
{
  "id": "201a9170-df7c-4cf7-b37f-672e0c548a98",
  "name": "Serg Metelin",
  "email": "serg.metelin@gmail.com"
}
```

# Images

## Get images for current user

> Definition 

> `GET /user/images`

```json
{
  "data": [
    {
      "id": "72e81a1f-fb0a-4caf-822a-6b7c90bd7411",
      "created_at": "2016-12-04T06:34:37.650Z",
      "url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/72e81a1f-fb0a-4caf-822a-6b7c90bd7411/5b9a5023-1249-41a3-b94a-0dd716b55763.jpg",
      "secure_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/72e81a1f-fb0a-4caf-822a-6b7c90bd7411/5b9a5023-1249-41a3-b94a-0dd716b55763.jpg",
      "secure_thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/72e81a1f-fb0a-4caf-822a-6b7c90bd7411/thumb_5b9a5023-1249-41a3-b94a-0dd716b55763.jpg",
      "thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/72e81a1f-fb0a-4caf-822a-6b7c90bd7411/thumb_5b9a5023-1249-41a3-b94a-0dd716b55763.jpg"
    },
    {
      "id": "926f0fe3-b92b-4ff0-8fc8-eaa76c234304",
      "created_at": "2016-12-04T06:34:38.733Z",
      "url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/926f0fe3-b92b-4ff0-8fc8-eaa76c234304/6bbb8182-b19c-4c74-b41e-adb6bd4166cb.jpg",
      "secure_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/926f0fe3-b92b-4ff0-8fc8-eaa76c234304/6bbb8182-b19c-4c74-b41e-adb6bd4166cb.jpg",
      "secure_thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/926f0fe3-b92b-4ff0-8fc8-eaa76c234304/thumb_6bbb8182-b19c-4c74-b41e-adb6bd4166cb.jpg",
      "thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/926f0fe3-b92b-4ff0-8fc8-eaa76c234304/thumb_6bbb8182-b19c-4c74-b41e-adb6bd4166cb.jpg"
    }]
}
```

## Upload and process a bulk of images

Progimage allows uploading either one image or a bulk of images. If you are planning to upload only one image, send a `process` object with parameters of the image. If you are planning to upload bulk, send an array of object with parameters for each image.

Progimage allows upload and manipulation of images in 3 ways: via base64 encoded image, via a url to a remote file or referencing existing Progimage image. Please refer to parameters below to identify which parameters you need to use.

> `POST /user/images`

> Example request (bulk upload):

```json
    {
        "process": [{
            "remote_file_url": "https://support.files.wordpress.com/2009/07/pigeony.jpg?w=688", 
            "rotation":"90", 
            "quality":"90", 
            "thumb_w": "100", 
            "thumb_h": "300", 
            "filter": "gotham", 
            "round_corners": "25"
            },{
            "remote_file_url": "https://support.files.wordpress.com/2009/07/pigeony.jpg?w=688", 
            "rotation":"90", 
            "quality":"90", 
            "thumb_w": "100", 
            "thumb_h": "300", 
            "filter": "gotham", 
            "round_corners": "25"
            }]
    }
```

> Example Response (bulk upload):

```json
{
  "data": [
    {
      "id": "8d1863c2-5f7f-4d4b-b9c9-e283bf0c1de2",
      "created_at": "2016-12-05T00:35:15.345Z",
      "url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/8d1863c2-5f7f-4d4b-b9c9-e283bf0c1de2/af3922ba-464f-4a6f-96d4-eff68a820e5b.jpg",
      "secure_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/8d1863c2-5f7f-4d4b-b9c9-e283bf0c1de2/af3922ba-464f-4a6f-96d4-eff68a820e5b.jpg",
      "secure_thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/8d1863c2-5f7f-4d4b-b9c9-e283bf0c1de2/thumb_af3922ba-464f-4a6f-96d4-eff68a820e5b.jpg",
      "thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/8d1863c2-5f7f-4d4b-b9c9-e283bf0c1de2/thumb_af3922ba-464f-4a6f-96d4-eff68a820e5b.jpg"
    },
    {
      "id": "febceea8-0c9c-443a-a595-164bb5c400ad",
      "created_at": "2016-12-05T00:35:16.700Z",
      "url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/febceea8-0c9c-443a-a595-164bb5c400ad/c415c293-03ff-4229-8035-b4e2331db586.jpg",
      "secure_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/febceea8-0c9c-443a-a595-164bb5c400ad/c415c293-03ff-4229-8035-b4e2331db586.jpg",
      "secure_thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/febceea8-0c9c-443a-a595-164bb5c400ad/thumb_c415c293-03ff-4229-8035-b4e2331db586.jpg",
      "thumbnail_url": "http://d1m92amay62gq7.cloudfront.net/uploads/image/file/febceea8-0c9c-443a-a595-164bb5c400ad/thumb_c415c293-03ff-4229-8035-b4e2331db586.jpg"
    }
  ]
}
```

> Example request (single upload):

```json
    {
        "process": {
            "remote_file_url": "https://support.files.wordpress.com/2009/07/pigeony.jpg?w=688", 
            "rotation":"90", 
            "quality":"90", 
            "thumb_w": "100", 
            "thumb_h": "300", 
            "filter": "gotham", 
            "round_corners": "25"
            }
    }
```

> Example response (single upload):

```json
{
  "data": {
    "id": "8646ceca-9f7a-4b3e-99cd-50968a96db4a",
    "user_id": "201a9170-df7c-4cf7-b37f-672e0c548a98",
    "file": {
      "url": "/uploads/image/file/8646ceca-9f7a-4b3e-99cd-50968a96db4a/2a522386-5369-4f4b-8326-5a70fa9f964f.jpg",
      "mini": {
        "url": "/uploads/image/file/8646ceca-9f7a-4b3e-99cd-50968a96db4a/mini_2a522386-5369-4f4b-8326-5a70fa9f964f.jpg"
      },
      "thumb": {
        "url": "/uploads/image/file/8646ceca-9f7a-4b3e-99cd-50968a96db4a/thumb_2a522386-5369-4f4b-8326-5a70fa9f964f.jpg"
      }
    },
    "created_at": "2016-12-04T06:51:00.981Z",
    "updated_at": "2016-12-04T06:51:00.981Z"
  }
}
```

#### Root Parameters
            
    Name | Type | Is Required | Description
    -------------- | -------------- | -------------- | --------------
    process | hash or array | YES | A root image hash or array of image hashes

#### Image Hash Parameters

    Name | Type | Is Required | Description
    -------------- | -------------- | -------------- | --------------
    remote_file_url | string | NO* | URL of remote image
    file | string | NO* | Base64 encoded representation of an image
    image_id | string | NO* | ID of an existing Progimage image
    quality | integer | NO | Optional parameter to compress the image - quality of the image ranges 0..100
    thumb_w | integer | NO | Optional parameter for specific width of the thumbnail
    thumb_h | integer | NO | Optional parameter for specific height of the thumbnail
    rotation |integer | NO | Optional parameter for rotation angle 
    filter | string | NO | Optional parameter to apply the filter. Possible filters: `toaster, lomo, kelvin, colortone, gotham` 
    round_corners | integer | NO | Optional parameter to set the width of round corners on the image, value should be > 0

<aside class="danger">
* At least one of the parameters, `remote_file_url`, `image_id` or `file` is required
</aside>
