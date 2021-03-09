# Authentication

The API uses [OAuth 2.0](https://oauth.net/2/) as protocol for authentication.
It officially implements the [`implicit`](https://oauth.net/2/grant-types/implicit/) and [`password`](https://oauth.net/2/grant-types/password/) grant type

## `implicit` grant

### Receive a token

1. Make a GET request to `http://oauth-server/authorize?client_id=web&redirect_uri=http://localhost:4200&response_type=token`
2. User will be redirected to login form where he has to specify his email and password
3. After successful authentication user will be redirected to a previously specified uri (i.e. _http://localhost:4200)._ The generated token will be added as a fragment string

### Verify a token

1. 

```sh
curl -X POST \
  http://oauth-server/check-token \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F token=TOKEN_HERE
```

2. As a response will be either valid token's json representation or an error

## `password` grant

### Receive a token

```sh
curl -X POST \
  http://oauth-server/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=password&username=EMAIL&password=PASSWORD'
```

### Refresh token

```sh
curl -X POST \
  http://oauth-server/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=refresh_token&refresh_token=REFRESH_TOKEN'
```
