# Snapscreen API documentation

Table of Contents
=================
* [A REST service which serves OAuth2 token requests (`/oauth/token`).](#oauthtoken)
* [A REST service which serves search in the index of TV channels grabbed by the system (`/tv-search`).](#tv-search)
* [A REST service which serves TS images grabbed by the system on TV channels (`/ts-images`).](#ts-images)
* [A REST service which serves search in the index of advertisements (`/ads/search`).](#adssearch)
* [A REST service which serves ad images (`/ads/images`).](#adsimages)
* [Support.](#support)

## `/oauth/token`
A REST service which serves OAuth2 token requests (/oauth/token).

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=client_credentials&device_fingerprint)`
#### Description
Grants an access token by client credentials.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | |
| client_secret | form | The client secret. | No | string | | 
| grant_type | form | Access grant type, in this case should client_credentials. | Yes | string (strict value: client_credentials) | | 
| device_fingerprint | form | Unique device fingerprint. | Yes | String | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema:
```javascript
{
  "access_token": string
  "token_type": string
  "refresh_token": string
  "expires_in": number (long)
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema:
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=anonymous&email&device_fingerprint)`

#### Description
Grants an access token to an anonymous user.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | |
| client_secret | form | The client secret. | Yes | string | | 
| grant_type | form | Access grant type, in this case should anonymous. | Yes | string (strict value: anonymous) | |
| email | form | Anonymous user email address if available. The main purpose of this is an authentication of the test mobile application. | No | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | string | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema:
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema:  
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=password&username&password&device_fingerprint)`

#### Description
Grants an access token to a user by email and password.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | |
| client_secret | form | The client secret. | Yes | string | |
| grant_type | form | Access grant type, in this case should password. | Yes | string (strict value: password) | |
| username | form | A user email. | Yes| string | |
| password | form | A user password. | Yes | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | string | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema: 
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema: 
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=facebook&access_token&anonymous_token&device_fingerprint)`

#### Description
Grants an access token to a user via Facebook.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | | 
| client_secret | form | The client secret. | Yes | string | |
| grant_type | form | Access grant type, in this case should facebook. | Yes | string (strict value: facebook) | |
| access_token | form | Facebook OAuth access token to verify user identity. | Yes | string | | 
| anonymous_token | form | Access token granted to anonymous user, if any available. | No | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | string | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. 
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema: 
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=google&id_token&access_token&anonymous_token&device_fingerprint)`

#### Description
Grants an access token to a user via Google.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | | 
| client_secret | form | The client secret. | Yes | string | |
| grant_type | form | Access grant type, in this case should google. | Yes | string (strict value: google) | | 
| id_token | form | Google ID token to verify user identity. | Yes | string | |
| access_token | form | Google OAuth access token to verify user identity. Will only be used to retrieve user profile image. For authentication only id_token is required. | No | string | |
| anonymous_token | form | Access token granted to anonymous user, if any available. | No | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | String | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema: 
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema: 
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=twitter&x_auth_service_provider&x_verify_credentials_authorization&anonymous_token&device_fingerprint)`

#### Description
Grants an access token to a user via Twitter.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | |
| client_secret | form | The client secret. | Yes | string | |
| grant_type | form | Access grant type, in this case should twitter. | Yes | string (strict value: twitter) | |
| x_auth_service_provider | form | Twitter OAuth Echo service provider URL. | Yes | string | |
| x_verify_credentials_authorization | form | Twitter OAuth Echo authorization header. | Yes | string | |
| anonymous_token | form | Access token granted to anonymous user, if any available. | No | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | String | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema: 
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema: 
```javascript
{
  "error": string,
  "error_description": string
}
```

### `POST https://api.snapscreen.com/api/oauth/token (client_id&client_secret&grant_type=refresh_token&refresh_token&device_fingerprint)`

#### Description
Refreshes an access token.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | | 
| client_secret | form | The client secret. | Yes | string | | 
| grant_type | form | Access grant type, in this case should refresh_token. | Yes | string (strict value: refresh_token) | | 
| refresh_token | form | Refresh token which was provided after access grant. | Yes | string | |
| device_fingerprint | form | Unique device fingerprint. | Yes | String | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An access token and it requisites. Schema: 
```javascript
{
  "access_token": string,
  "token_type": string,
  "refresh_token": string,
  "expires_in": number (long),
  "scope": string
}
```
* Status code: **4xx, 5xx**. Description: An error during authentication. Schema: 
```javascript
{
  "error": string,
  "error_description": string
}
```

### `DELETE https://api.snapscreen.com/api/oauth/token`

#### Description
Revokes an access token.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| AccessToken | header | Access token which was provided after access grant. | Yes | string | |

#### Responses
* Status code: **200**. Description: Access token revoked.

## `/tv-search`
A REST service which serves search in the index of TV channels grabbed by the system (`/tv-search`).

### `POST https://api.snapscreen.com/api/tv-search/epg`

#### Description
Searches the EPG data using the TV index by a fingerprint optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made this request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make this request for search. | No | string | |
| | body | The data of the computed fingerprint. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
           "href": string (url)
          }
        }
      },
      "epgUnit": {
        "id": number (long),
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "title": string,
        "subtitle": string,
        "description": string,
        "broadcastDate": string (date: yyyy-MM-dd),
        "productionDate": string (date: yyyy-MM-dd),
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "credits": {
          "actor": [
            {
              "name": string,
              "role": string
            }
          ],
          "director": [
            {
              "name": string
            }
          ]
        },
        "genres": array (string),
        "bannerUrl": string (url),
        "posterUrl": string (url),
        "keywords": string,
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          },
          "bannerThumbnail": {
            "href": string (url)
          },
          "posterThumbnail": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long)
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long)
      "score": number (double)
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/epg/by-image`

#### Description
Searches the EPG data using the TV index by an image optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Width | header | The width of the image for search. | Yes | number (int) | |
| X-Snapscreen-Height | header | The height of the image for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the image. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
           "href": string (url)
          }
        }
      },
      "epgUnit": {
        "id": number (long),
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "title": string,
        "subtitle": string,
        "description": string,
        "broadcastDate": string (date: yyyy-MM-dd),
        "productionDate": string (date: yyyy-MM-dd),
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "credits": {
          "actor": [
            {
              "name": string,
              "role": string
            }
          ],
          "director": [
            {
              "name": string
            }
          ]
        },
        "genres": array (string),
        "bannerUrl": string (url),
        "posterUrl": string (url),
        "keywords": string,
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          },
          "bannerThumbnail": {
            "href": string (url)
          },
          "posterThumbnail": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long)
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long)
      "score": number (double)
    }
  ],
  "screenQuadrangles": [
    {
      "a": {
        "x": number (int),
        "y": number (int)
      },
      "b": {
        "x": number (int),
        "y": number (int)
      },
      "c": {
        "x": number (int),
        "y": number (int)
      },
      "d": {
        "x": number (int),
        "y": number (int)
      }
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/epg/near-timestamp`

#### Description
Searches the EPG data using the TV index by a fingerprint and near the specified timestamp optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing to search around the timestamp. Optional, if not provided the default value be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the computed fingerprint. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
           "href": string (url)
          }
        }
      },
      "epgUnit": {
        "id": number (long),
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "title": string,
        "subtitle": string,
        "description": string,
        "broadcastDate": string (date: yyyy-MM-dd),
        "productionDate": string (date: yyyy-MM-dd),
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "credits": {
          "actor": [
            {
              "name": string,
              "role": string
            }
          ],
          "director": [
            {
              "name": string
            }
          ]
        },
        "genres": array (string),
        "bannerUrl": string (url),
        "posterUrl": string (url),
        "keywords": string,
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          },
          "bannerThumbnail": {
            "href": string (url)
          },
          "posterThumbnail": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long)
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long)
      "score": number (double)
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/epg/near-timestamp/by-image`

#### Description
Searches the EPG data using the TV index by an image and near the specified timestamp optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Width | header | The width of the image for search. | Yes | number (int) | |
| X-Snapscreen-Height | header | The height of the image for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing to search around the timestamp. Optional, if not provided the default value will be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the image. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
           "href": string (url)
          }
        }
      },
      "epgUnit": {
        "id": number (long),
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "title": string,
        "subtitle": string,
        "description": string,
        "broadcastDate": string (date: yyyy-MM-dd),
        "productionDate": string (date: yyyy-MM-dd),
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "credits": {
          "actor": [
            {
              "name": string,
              "role": string
            }
          ],
          "director": [
            {
              "name": string
            }
          ]
        },
        "genres": array (string),
        "bannerUrl": string (url),
        "posterUrl": string (url),
        "keywords": string,
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          },
          "bannerThumbnail": {
            "href": string (url)
          },
          "posterThumbnail": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long)
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long)
      "score": number (double)
    }
  ],
  "screenQuadrangles": [
    {
      "a": {
        "x": number (int),
        "y": number (int)
      },
      "b": {
        "x": number (int),
        "y": number (int)
      },
      "c": {
        "x": number (int),
        "y": number (int)
      },
      "d": {
        "x": number (int),
        "y": number (int)
      }
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/sport`

#### Description
Searches sport events using the TV index by a fingerprint optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made this request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make this request for search. | No | string | |
| | body | The data of the computed fingerprint. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
            "href": string (url)
          }
        }
      },
      "sportEvent": {
        "id": number (long),
        "sportDataProviderCode": string,
        "sportDataProviderMatchId": string,
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "sport": string,
        "tournament": string,
        "category": string,
        "competitors": [
          {
             "name": string
          }
        ],
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long),
      "score": number (double)
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/sport/by-image`

#### Description
Searches sport events using the TV index by an image optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Width | header | The width of the image for search. | Yes | number (int) | |
| X-Snapscreen-Height | header | The height of the image for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the image. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
            "href": string (url)
          }
        }
      },
      "sportEvent": {
        "id": number (long),
        "sportDataProviderCode": string,
        "sportDataProviderMatchId": string,
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "sport": string,
        "tournament": string,
        "category": string,
        "competitors": [
          {
             "name": string
          }
        ],
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long),
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ],
  "screenQuadrangles": [
    {
      "a": {
        "x": number (int),
        "y": number (int)
      },
      "b": {
        "x": number (int),
        "y": number (int)
      },
      "c": {
        "x": number (int),
        "y": number (int)
      },
      "d": {
        "x": number (int),
        "y": number (int)
      }
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/sport/near-timestamp`

#### Description
Searches sport events using the TV index by a fingerprint and near the specified timestamp optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing to search around the timestamp. Optional, if not provided the default value be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the computed fingerprint. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
            "href": string (url)
          }
        }
      },
      "sportEvent": {
        "id": number (long),
        "sportDataProviderCode": string,
        "sportDataProviderMatchId": string,
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "sport": string,
        "tournament": string,
        "category": string,
        "competitors": [
          {
             "name": string
          }
        ],
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long),
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/tv-search/sport/near-timestamp/by-image`

#### Description
Searches sport events using the TV index by an image and near the specified timestamp optionally filtering by the ISO-code of a country in which TV channels broadcasted.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Width | header | The width of the image for search. | Yes | number (int) | |
| X-Snapscreen-Height | header | The height of the image for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing to search around the timestamp. Optional, if not provided the default value will be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
| X-Snapscreen-CountryCode | header | The ISO code of a country to filter result entries for TV channels broadcasted in it. | No | string | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the image. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "tvChannel": {
        "id": number (long),
        "code": string,
        "name": string,
        "homepage": string (url),
        "grabbed": boolean,
        "tvCategories": [
          {
            "id": string,
            "name": string,
            "description": string
          }
        ],
        "language": {
          "id": number (long),
          "code": string,
          "name": string
        },
        "_links": {
          "self": {
            "href": string (url)
          },
          "logo": {
            "href": string (url)
          },
          "poster": {
            "href": string (url)
          }
        }
      },
      "sportEvent": {
        "id": number (long),
        "sportDataProviderCode": string,
        "sportDataProviderMatchId": string,
        "tvChannelId": number (long),
        "startTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "endTime": string (iso date-time: yyyy-MM-dd'T'HH:mm:ss.SSSZZ),
        "sport": string,
        "tournament": string,
        "category": string,
        "competitors": [
          {
             "name": string
          }
        ],
        "_links": {
          "self": {
            "href": string (url)
          },
          "tvChannel": {
            "href": string (url)
          },
          "tvChannelLogo": {
            "href": string (url)
          }
        }
      },
      "timestampRef": number (long),
      "score": number (double),
    }
  ],
  "adEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ],
  "screenQuadrangles": [
    {
      "a": {
        "x": number (int),
        "y": number (int)
      },
      "b": {
        "x": number (int),
        "y": number (int)
      },
      "c": {
        "x": number (int),
        "y": number (int)
      },
      "d": {
        "x": number (int),
        "y": number (int)
      }
    }
  ]
}
```

## `/ts-images`
A REST service which serves TS images grabbed by the system on TV channels (/ts-images).

### `GET /ts-images/{tvChannelId}/{timestampRef}`
#### Description
Gets the metadata of a TS image grabbed on a TV channel at a timestamp.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| tvChannelId | path | The id of a TV channel on which a TS image was grabbed. | Yes | number (long) | |
| timestampRef | path | The timestamp when a TS image was grabbed. | Yes | number (long) | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: A TS image. Schema:
```javascript
{
  id: number (long),
  tvChannelId: number (long),
  timestampRef: number (long),
  mimeType: string (MIME Type),
  width: number (int),
  height: number (int),
  _links: {
    self: {
      href: string (url)
    },
    download: {
      href: string (url)
    }
  }
}
```

### `GET /ts-images/{tvChannelId}/{timestampRef}/download`
#### Description
Downloads the data of a TS image grabbed on a TV channel at a timestamp.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| tvChannelId | path | The id of a TV channel on which a TS image was grabbed. | Yes | number (long) | |
| timestampRef | path | The timestamp when a TS image was grabbed. | Yes | number (long) | | 

#### Produces
* The MIME Type of the TS image (e.g. image/jpeg).

#### Responses
* Status code: **200**. Description: The data of the TS image. Filename:
ts-image-{tvChannelId}-{timestampRef}.{mimeType.subType}.

## `/ads/search`
A REST service which serves search in the index of advertisements (/ads/search).

### `POST https://api.snapscreen.com/api/ads/search`
#### Description
Searches in the index by a fingerprint.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made this request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make this request for search. | No | string | |
| | body | The data of the computed fingerprint. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ]
}
```

### `POST https://api.snapscreen.com/api/ads/search/by-image`
#### Description
Searches in the index by an image optionally filtering.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Width | header | The width of the image for search. | Yes | number (int) | |
| X-Snapscreen-Height | header | The height of the image for search. | Yes | number (int) | |
| X-Snapscreen-GeoLocation | header | A GEO location from which the user made a request for search. | No | string | |
| X-Snapscreen-DeviceInfo | header | An info about a device used by the user to make a request for search. | No | string | |
| | body | The data of the image. | Yes | array (byte) | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The search result. Schema:
```javascript
{
  "requestUuid": string (UUID),
  "resultEntries": [
    {
      "advertisement": {
        "id": number (long),
        "title": string,
        "description": string,
        "landingPage": string (url),
        "duration": number (long)
      },
      "timeOffset": number (long),
      "score": number (double)
    }
  ],
  "screenQuadrangles": [
    {
      "a": {
        "x": number (int),
        "y": number (int)
      },
      "b": {
        "x": number (int),
        "y": number (int)
      },
      "c": {
        "x": number (int),
        "y": number (int)
      },
      "d": {
        "x": number (int),
        "y": number (int)
      }
    }
  ]
}
```

## `/ads/images`
A REST service which serves advertisement images. (/ads/images).

### `GET /ads/images/{advertisementId}/{timeOffset}`
#### Description
Retrieves an image of the given advertisement at the given time offset.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| advertisementId | path | The id of an advertisement whose image you want to retrieve. | Yes | number (long) | |
| timeOffset | path | The time offset of the image you want to retrieve. | Yes | number (long) | | 

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: An advertisement image. Schema:
```javascript
{
  id: number (long),
  advertisementId: number (long),
  timeOffset: number (long),
  mimeType: string (MIME Type),
  width: number (int),
  height: number (int),
  _links: {
    self: {
      href: string (url)
    },
    advertisement: {
      href: string (url)
    },
    download: {
      href: string (url)
    }
  }
}
```

### `GET /ads/images/{advertisementId}/{timeOffset}/download`
#### Description
Downloads the content of an image of the given advertisement at the given time offset.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| advertisementId | path | The id of an advertisement whose image you want to retrieve. | Yes | number (long) | |
| timeOffset | path | The time offset of the image you want to retrieve. | Yes | number (long) | | 

#### Produces
* The MIME Type of the advertisement image (e.g. image/jpeg).

#### Responses
* Status code: **200**. Description: The data of the advertisement image.
Filename: ad-image-{id}-{advertisementId}-{timeOffset}.{mimeType.subType}.

## Support
In case of any questions or problems please contact us at [support@snapscreen.com](mailto:support@snapscreen.com).