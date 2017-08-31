# Snapscreen API documentation

## `/oauth/token`
A REST service which serves OAuth2 token requests (/oauth/token).

### `POST /oauth/token (client_id&client_secret&grant_type=client_credentials&device_fingerprint)`
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

### `POST /oauth/token (client_id&client_secret&grant_type=anonymous&email&device_fingerprint)`

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
  "error_description: string
}
```

### `POST /oauth/token (client_id&client_secret&grant_type=password&username&password&device_fingerprint)`

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

### `POST /oauth/token (client_id&client_secret&grant_type=facebook&access_token&anonymous_token&device_fingerprint)`

#### Description
Grants an access token to a user via Facebook.

#### Security
Client credentials required.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| client_id | form | The client identifier specific to the client. | Yes | string | | 
| client_secret | form | The client secret. | Yes | string | |
| grant_type | form | Access grant type, in this case should facebook. | Yes | string (strict value: facebook) 
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

### `POST /oauth/token (client_id&client_secret&grant_type=google&id_token&access_token&anonymous_token&device_fingerprint)`

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

### `POST /oauth/token (client_id&client_secret&grant_type=twitter&x_auth_service_provider&x_verify_credentials_authorization&anonymous_token&device_fingerprint)`

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

### `POST /oauth/token (client_id&client_secret&grant_type=refresh_token&refresh_token&device_fingerprint)`

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

### DELETE /oauth/token

#### Description
Revokes an access token.

#### Parameters
| Name | Located in | Description | Required | Schema | Default value |
| ---- | ---------- | ----------- | -------- | ------ | ------------- |
| AccessToken | header | Access token which was provided after access grant. | Yes | string | |

#### Responses
* Status code: **200**. Description: Access token revoked. 