
# Grant [![img-npm-version]][npm]

_**grant**_ is build on top of **[mashape][mashape] / [guardian][guardian]**


## Providers [live demo][demo]

| | | | | | |
:---: | :---: | :---: | :---: | :---: | :---:
[amazon]() | [asana](http://developer.asana.com/documentation/) | [bitly](http://dev.bitly.com) | [box](https://developers.box.com/) | [digitalocean](https://developers.digitalocean.com/) | [dropbox](https://www.dropbox.com/developers)
[facebook](https://developers.facebook.com) | [flickr](https://www.flickr.com/services/api/) | [foursquare](https://developer.foursquare.com/) | [github](http://developer.github.com) | [google](https://developers.google.com/) | [heroku](https://devcenter.heroku.com/categories/platform-api)
[imgur](https://api.imgur.com/) | [instagram](http://instagram.com/developer) | [linkedin](http://developer.linkedin.com) | [live](http://msdn.microsoft.com/en-us/library/dn783283.aspx) | [mailchimp](http://apidocs.mailchimp.com/) | [openstreetmap](http://wiki.openstreetmap.org/wiki/API_v0.6)
[paypal](https://developer.paypal.com/docs/) | [slack](https://api.slack.com/) | [soundcloud](http://developers.soundcloud.com) | [stackexchange](https://api.stackexchange.com) | [stocktwits](http://stocktwits.com/developers) | [stripe](https://stripe.com/docs)
[trello](https://trello.com/docs/) | [tumblr](http://www.tumblr.com/docs/en/api/v2) | [twitch](https://github.com/justintv/twitch-api) | [twitter](https://dev.twitter.com) | [vimeo](https://developer.vimeo.com/) | [yahoo](https://developer.yahoo.com/)


## Usage

```js
var express = require('express');
var Grant = require('grant');

var grant = new Grant({...configuration see below...});

var app = express();
// mount grant
app.use(grant);
// app server middlewares
app.use(cookieParser());
app.use(session());
```


#### Reserved Routes for Grant

```bash
/connect/:provider/:override?
/step/:number
/connect/:provider/callback
```


## Configuration

```js
{
  "server": {
    "protocol": "http",
    "host": "localhost:3000",
    "callback": "/callback"
  },
  "provider1": {
    "key": "...",
    "secret": "...",
    "scope": ["scope1", "scope2", ...],
    "state": "some state",
    "callback": "/provider1/callback"
  },
  "provider2": {...},
  ...
}
```

- **server** - configuration about your server
  - **protocol** - either `http` or `https`
  - **host** - your server's host name `localhost:3000` | `dummy.com:5000` | `mysite.com` ...
  - **callback** - common callback for all providers in your config
- **provider1** - any supported provider _(see the above table)_ `google` | `facebook` ...
  - **key** - `consumer_key` or `client_id` of your app
  - **secret** - `consumer_secret` or `client_secret` of your app
  - **scope** - OAuth scopes array
  - **state** - OAuth state string
  - **callback** - specific callback to use for this provider _(overrides the global one specified in the `server` key)_<br>
    - These callbacks are used only on your server!<br>
    - These callbacks are not the one you specify for your app!
    - You should always specify the _callback/redirect url_ of your app like this:<br>
    http(s)://mydomain.com/connect/[provider]/callback where<br>
      - _provider_ is one of the above provider names
      - _mydomain.com_ is your site's domain name
  - **protocol** | **host** - additionally you can override these common values inherited from the `server` key
  - **custom1** - create sub configuration for that provider<br>
    _You can override any of the above keys here_<br>
    _**Example**_<br>
    
    ```js
    "facebook": {
      "key": "...",
      "secret": "...",
      // by default request publish permissions via /connect/facebook
      "scope": ["publish_actions", "publish_stream"],
      // set specific callback route on your server for this provider only
      "callback": "/facebook/callback"
      // custom override keys
      "groups": {
        // request only group permissions via /connect/facebook/groups
        "scope": ["user_groups", "friends_groups"]
      },
      "pages": {
        // request only page permissions via /connect/facebook/pages
        "scope": ["manage_pages"],
        // additionally use specific callback route on your server for this override only
        "callback": "/pages/callback"
      }
    }
    ```


## Typical Flow

1. Register OAuth application on your provider's web site
2. For `callback/redirect uri` you should always use this format<br>
  http(s)://mydomain.com/connect/[provider]/callback where<br>
  - _provider_ is one of the above provider names
  - _mydomain.com_ is your site's domain name
3. Set up your common server `callback` in _server.json_ This is the final callback when the OAuth flow is complete. Grant will redirect you to it after hitting the `/connect/[provider]/callback` specified for your app, therefore this _callback_ should be something different _(take a look at the [reserved routes][routes] for Grant)_
3. Optionally you can override the end _callback_ for each provider individually _(take a look at the [configuration][configuration] structure)_


## License

MIT


  [mashape]: https://www.mashape.com/
  [guardian]: http://guardianjs.com/
  [bible]: http://oauthbible.com/
  [demo]: http://grant-oauth.herokuapp.com/
  [npm]: https://www.npmjs.org/package/grant

  [img-npm-version]: http://img.shields.io/npm/v/grant.svg?style=flat (NPM Version)
  [img-npm-downloads]: http://img.shields.io/npm/dm/grant.svg?style=flat (NPM Downloads)

  [routes]: #reserved-routes-for-grant
  [configuration]: #configuration
