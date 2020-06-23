![Build Status](https://github.com/cardinalby/google-api-fetch-token-action/workflows/build-test/badge.svg)

# The purpose

The action fetches access token from Google API. 

It can be used to **prevent your Refresh Token from being expired**.

According to [Google's guide](https://developers.google.com/identity/protocols/oauth2#expiration), 
the refresh token might stop working if it has not been used for six months.

If you use it in your workflow running less often than 6 months, you can face the problem. 
[Schedule](https://help.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events-schedule) 
this action to periodically access Google API and fetch access token to avoid that.

Based on [typed-chrome-webstore-api](https://www.npmjs.com/package/typed-chrome-webstore-api) package.

## About Google API access
To setup API access you need to specify `clientId`, `clientSecret` and `refreshToken` inputs.
To find out how to obtain them you can read:
* [Using the Chrome Web Store Publish API](https://developer.chrome.com/webstore/using_webstore_api) 
* [How to generate Google API keys](https://github.com/DrewML/chrome-webstore-upload/blob/master/How%20to%20generate%20Google%20API%20keys.md)

## Inputs

* `clientId` **Required**
* `clientSecret` **Required**
* `refreshToken` **Required**

Don't forget to store these values in secrets!

## Outputs
* `accessToken` fetched access token

## Example usage()
Create a separate workflow with scheduled job:
```yaml
name: "fetch-access-token"
on:
  schedule:
    - cron:  '* * * */1 *' # run every month

jobs:
  fetchToken:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]
    steps:
      - uses: cardinalby/google-api-fetch-token-action@v1
        with:
          clientId: ${{ secrets.G_API_CLIENT_ID }}
          clientSecret: ${{ secrets.G_API_CLIENT_SECRET }}
          refreshToken: ${{ secrets.G_API_REFRESH_TOKEN }}
```
