# Payment Redirect with Open Payments API

Install dependencies
```bash
pnpm install
```

[Interactive Grant Example](https://github.com/interledger/open-payments-example)
```bash
# Create private key
# https://wallet.interledger-test.dev/settings/developer-keys
touch ih2024-eur.key

# Set environment
cp .env.sample.sh .env

# Run example
node index.js
```

Refactored above example into the two scripts listed below:

## redirect.js

Env input
- APP_IN_WALLET_ADDRESS_URL
- APP_OUT_WALLET_ADDRESS_URL
- APP_KEY_ID
- APP_PRIVATE_KEY
- APP_SUCCESS_URL

Console output
```javascript
{
  "Redirect": outgoingPaymentGrant.interact.redirect,
  "ContinueURI": outgoingPaymentGrant.continue.uri,
  "AccessToken": outgoingPaymentGrant.continue.access_token.value,
}
```

ASE will redirect to e.g.
```
https://localhost:8443/api/admin/webhook/ih2024?result=grant_rejected
https://localhost:8443/api/admin/webhook/ih2024?result=grant_accepted
```

## continue.js

Env input
- APP_IN_WALLET_ADDRESS_URL
- APP_KEY_ID
- APP_PRIVATE_KEY
- APP_CONTINUE_URI
- APP_CONTINUE_ACCESS_TOKEN

Console output
```javascript
{
  "InteractionResult": acceptOrReject,
  "PaymentSuccess": trueOrFalse,
  "Error": errorMessage
}
```

## Two step interaction

```bash
conf # Set config in .env
node redirect.js
# Copy and paste config values from script output

conf continue # Set config in .env.continue.sh
node continue.js
# Below because no server is running at webhook URL yet?
# "There was an error continuing the grant.
# You probably have not accepted the grant at the url
# (or it has already been used up, in which case, rerun the script)."
```
