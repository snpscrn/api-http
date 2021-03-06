# Snapscreen API documentation

Table of Contents
=================
* [Support.](#support)
* [A REST service which serves OAuth2 token requests (`/oauth/token`).](#oauthtoken)
* [A REST service which serves search in the index of TV channels grabbed by the system (`/tv-search`).](#tv-search)
* [A REST service which serves TS images grabbed by the system on TV channels (`/ts-images`).](#ts-images)
* [A REST service which serves search in the index of advertisements (`/ads/search`).](#adssearch)
* [A REST service which serves ad images (`/ads/images`).](#adsimages)
* [A REST service which serves TV channels (`/tv-channels`).](#tv-channels)
* [A REST service which serves created clips (`/clips`).](#clips)

## Support
In case of any questions or problems please contact us at [support@snapscreen.com](mailto:support@snapscreen.com).

## `/oauth/token`
A REST service which serves OAuth2 token requests (/oauth/token).

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
Searches the EPG data using the TV index by a fingerprint.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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

### `POST https://api.snapscreen.com/api/tv-search/epg/by-image`

#### Description
Searches the EPG data using the TV index by an image.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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
Searches the EPG data using the TV index by a fingerprint and near the specified timestamp.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp (the number of milliseconds from the epoch of 1970-01-01T00:00:00Z) to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing in milliseconds to search around the timestamp. Optional, if not provided the default value be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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

### `POST https://api.snapscreen.com/api/tv-search/epg/near-timestamp/by-image`

#### Description
Searches the EPG data using the TV index by an image and near the specified timestamp.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Timestamp | header | A timestamp (the number of milliseconds from the epoch of 1970-01-01T00:00:00Z) to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing in milliseconds to search around the timestamp. Optional, if not provided the default value will be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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
Searches sport events using the TV index by a fingerprint.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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
Searches sport events using the TV index by an image.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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
Searches sport events using the TV index by a fingerprint and near the specified timestamp.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-FingerprintAlgorithm | header | The version of an algorithm used to compute the fingerprint for search. | Yes | number (int) | |
| X-Snapscreen-Timestamp | header | A timestamp (the number of milliseconds from the epoch of 1970-01-01T00:00:00Z) to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing in milliseconds to search around the timestamp. Optional, if not provided the default value be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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

### `POST https://api.snapscreen.com/api/tv-search/sport/near-timestamp/by-image`

#### Description
Searches sport events using the TV index by an image and near the specified timestamp.

#### Security
Authentication required to have access to this resource.

#### Consumes
* application/octet-stream

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| X-Snapscreen-MimeType | header | The MIME type of the image for search. | Yes | string | |
| X-Snapscreen-Timestamp | header | A timestamp (the number of milliseconds from the epoch of 1970-01-01T00:00:00Z) to search near it. | Yes | number (long) | |
| X-Snapscreen-TimestampWing | header | A wing in milliseconds to search around the timestamp. Optional, if not provided the default value will be used. | No | number (long) | 30000 |
| X-Snapscreen-SearchAds | header | Whether to include ads into the result. | No | boolean | false |
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

## `/tv-channels`
A REST service which serves TV channels (/tv-channels).

### `GET /tv-channels{?grabbed=true,page,size,sort}`
#### Description
Lists grabbed TV channels registered in the system.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| grabbed | query | The flag indicating the system grabs TV channels. | Yes | boolean | true |
| page | query | The number of a page you want to retrieve. | No | number (int) | 0 |
| size | query | The size of a page you want to retrieve. | No | number (int) | 10 |
| sort | query | Properties that should be sorted by. Default sort direction is ascending. Use multiple sort parameters if you want to switch directions. | No | string (property,property(,ASC&#124;DESC)) | name |

#### Produces<
* application/json

#### Responses
* Status code: **200**. Description: A page of TV channels. Schema:
```javascript
 {
  _links: {
    self: {
      href: string (url)
    },
    next: {
        href: string (url)
    },
    prev: {
        href: string (url)
    }
  },
  _embedded: {
    tvChannelList: [
      {
        id: number (long),
        code: string,
        name: string,
        homepage: string (url),
        grabbed: boolean,
        tvCategories: [
          {
            id: string,
            name: string,
            description: string
          }
        ],
        language: {
          id: number (long),
          code: string,
          name: string
        },
        // begin: not accessible for customers
        epgProvider: {
          id: number (long),
          code: string,
          description: string
        },
        epgCode: string,
        sportEventProvider: {
          id: number (long),
          code: string,
          description: string
        },
        sportEventCode: string,
        // end: not accessible for customers
        _links: {
          self: {
            href: string (url)
          },
          logo: {
            href: string (url)
          },
          poster: {
            href: string (url)
          }
        }
      }
    ]
  },
  page: {
    size: number (int),
    totalElements: number (long),
    totalPages: number (int),
    number: number (int)
  }
}
```

## `/clips`
A REST service which serves created clips (/clips).

### `GET https://clip.snapscreen.com/api/clips/{clipId}`
#### Description
Retrieves metadata of the clip with the specified id.

#### Security
Authentication required to have access to this resource.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| clipId | path | The id of the clip. | Yes | string | |

#### Produces
* application/json

#### Responses
* Status code: **200**. Description: The clip metadata. Schema:
```javascript
{
  id: string,
  tvChannelId: number (long),
  timestampRefFrom: number (long),
  timestampRefTo: number (long),
  timestampRefThumb: number (long),
  _links: {
    self: {
      href: string (url)
    },
    thumbnail: {
      href: string (url)
    },
    video: {
        href: string (url)
    },
    player: {
        href: string (url)
    }
  }
}
```
* Status code: **404**. Description: Clip not found.. Schema:
