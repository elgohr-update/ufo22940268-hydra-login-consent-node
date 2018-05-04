# hydra-user-and-consent-provider-node

This is a reference implementation for the User Login and Consent flow of ORY Hydra version 1.0.x in NodeJS. The
application is bootstrapped using the `express` cli.

Apart from additions (`./routes/login.js`, `./routes/consent.js`) and their respective templates, only a [CSRF Middleware]
has been added. Everything else is the standard express template.

Also, a simple helper that makes HTTP requests has been added to `./services/hydra.js` which uses the `node-fetch`
library.

To set this example up with ORY Hydra, please refer to the [official documentation](https://www.ory.sh/docs).

## Running locally

To run this example locally, you will first want to start ORY Hydra. Please note, that the set up shown here might not
work in the future as it may become out of date. For now, this should work however. We are also assuming that you are
using ORY Hydra >= 1.0.0.

```
$ OAUTH2_CONSENT_URL=http://localhost:3000/consent \
    OAUTH2_LOGIN_URL=http://localhost:3000/login \
    OAUTH2_ISSUER_URL=http://localhost:4444 \
    DATABASE_URL=memory \
    hydra serve --dangerous-force-http
```

Next, you will need to create a new client that we can use to perform the OAuth 2.0 Authorization Code Flow:

```
$ hydra clients create \
    --endpoint http://localhost:4444 \
    --id test-client \
    --secret test-secret \
    --response-types code,id_token \
    --grant-types refresh_token,authorization_code \
    --scope openid,offline \
    --callbacks http://localhost:4445/callback
```

Now, run this project

```
$ npm i
$ HYDRA_URL=http://localhost:4444 npm start
```

And finally, initiate the OAuth 2.0 Authorization Code Flow:

```
$ hydra token user \
    --endpoint http://localhost:4444/ \
    --scope openid,offline \
    --client-id test-client \
    --client-secret test-secret
```
