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
| client_id | form | The client identifier specific to the client. | Yes | string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) | |
| client_secret | form | The client secret. | No | string | | 
| grant_type | form | Access grant type, in this case should client_credentials. | Yes | string (strict value: client_credentials) | | 
| device_fingerprint | form | Unique device fingerprint. | Yes | String | | 

#### Produces
* application/json

#### Responses
| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | An access token and it requisites. | `json` |
| | | `{` |
| | | `  access_token: string,` |
| | | `  token_type: string,` |
| | | `  refresh_token: string,` |
| | | `  expires_in: number (long),` |
| | | `  scope: string` |
| | | `}` |
| 4xx, 5xx | An error during authentication. |

```json
{
  error: string,
  error_description: string
}
```



### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=anonymous&amp;email&amp;device_fingerprint)

#### Description
Grants an access token to an anonymous user.

#### Security
Client credentials required.

#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should anonymous. 
| Yes 
| string (strict value: anonymous) 
<td align="right"> 
</tr>
<tr>
| email 
| form 
| Anonymous user email address if available. The main purpose of this is an authentication of
the test mobile application. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| String 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=password&amp;username&amp;password
&amp;device_fingerprint)
#### Description
Grants an access token to a user by email and password.
#### Security
Client credentials required.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should password. 
| Yes 
| string (strict value: password) 
<td align="right"> 
</tr>
<tr>
| username 
| query 
| A user email. 
| form 
| string 
<td align="right"> 
</tr>
<tr>
| password 
| form 
| A user password. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| String 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=facebook&amp;access_token&amp;anonymous_token
&amp;device_fingerprint)
#### Description
Grants an access token to a user via Facebook.
#### Security
Client credentials required.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should facebook. 
| Yes 
| string (strict value: facebook) 
<td align="right"> 
</tr>
<tr>
| access_token 
| form 
| Facebook OAuth access token to verify user identity. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| anonymous_token 
| form 
| Access token granted to anonymous user, if any available. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| string 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=google&amp;id_token&amp;access_token&amp;anonymous_token
&amp;device_fingerprint)
#### Description
Grants an access token to a user via Google.
#### Security
Client credentials required.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should google. 
| Yes 
| string (strict value: google) 
<td align="right"> 
</tr>
<tr>
| id_token 
| form 
| Google ID token to verify user identity. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| access_token 
| form 
| Google OAuth access token to verify user identity. Will only be used to retrieve user profile image.
For authentication only id_token is required. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
<tr>
| anonymous_token 
| form 
| Access token granted to anonymous user, if any available. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| String 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=twitter&amp;x_auth_service_provider
&amp;x_verify_credentials_authorization&amp;anonymous_token&amp;device_fingerprint)
#### Description
Grants an access token to a user via Twitter.
#### Security
Client credentials required.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should twitter. 
| Yes 
| string (strict value: twitter) 
<td align="right"> 
</tr>
<tr>
| x_auth_service_provider 
| form 
| Twitter OAuth Echo service provider URL. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| x_verify_credentials_authorization 
| form 
| Twitter OAuth Echo authorization header. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| anonymous_token 
| form 
| Access token granted to anonymous user, if any available. 
| No 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| String 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### POST /oauth/token (client_id&amp;client_secret&amp;grant_type=refresh_token&amp;refresh_token
&amp;device_fingerprint)
#### Description
Refreshes an access token.
#### Security
Client credentials required.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| client_id 
| form 
| The client identifier specific to the client. 
| Yes 
| string (possible values: com.snapscreen.admin, com.snapscreen.backend, com.snapscreen.mobile) 
<td align="right"> 
</tr>
<tr>
| client_secret 
| form 
| The client secret. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| grant_type 
| form 
| Access grant type, in this case should refresh_token. 
| Yes 
| string (strict value: refresh_token) 
<td align="right"> 
</tr>
<tr>
| refresh_token 
| form 
| Refresh token which was provided after access grant. 
| Yes 
| string 
<td align="right"> 
</tr>
<tr>
| device_fingerprint 
| form 
| Unique device fingerprint. 
| Yes 
| String 
<td align="right"> 
</tr>
</table>
#### Produces
<ul>
<li>application/json</li>
</ul>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| An access token and it requisites. 
| 
<pre>
{
  access_token: string,
  token_type: string,
  refresh_token: string,
  expires_in: number (long),
  scope: string
}
</pre>
 
</tr>
<tr>
| 4xx, 5xx 
| An error during authentication. 
| 
<pre>
{
  error: string,
  error_description: string
}
</pre>
 
</tr>
</table>
### DELETE /oauth/token
#### Description
Revokes an access token.
#### Parameters
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Name 
| Located in 
| Description 
| Required 
| Schema 
| Default value 
</tr>
</thead>
<tr>
| AccessToken 
| header 
| Access token which was provided after access grant. 
| Yes 
| string 
<td align="right"> 
</tr>
</table>
#### Responses
<table width="100%" cellpadding="5">
<caption></caption>
<thead>
<tr align="left" style="background-color: #f7f7f7;">
| Code 
| Description 
| Schema 
</tr>
</thead>
<tr>
| 200 
| Access token revoked. 
|  
</tr>
</table>