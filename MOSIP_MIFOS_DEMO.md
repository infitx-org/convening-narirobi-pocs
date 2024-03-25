## MOSIP and MIFOS integration

- Assume the token is `abcdefghijklmnopqrstuvwxyz`
- Send post account call from payee pm4ml
```
curl 'http://mosippayee-sdk-scheme-adapter-api-svc:4001/accounts'
  -H 'content-type: application/json'
  -H 'accept-encoding: gzip, compress, deflate, br'
  --data-binary '[{"idType":"ALIAS","idValue":"abcdefghijklmnopqrstuvwxyz","currency":"EUR"}]'
```
- Map the token to MIFOS account number (e.g. 987) by sending the following request
```
curl --location 'https://pta-portal-mosippayee.devpm4ml.labspm4ml1002.mojaloop.live/tokens' \
--header 'Content-Type: application/json' \
--data '{
    "paymentToken": "abcdefghijklmnopqrstuvwxyz",
    "payeeIdType": "IBAN",
    "payeeId": "987"
}'
```
- Access payer TTK and load the test case `sdk_post_transfer.json` and environment file `switch-env.json`
- Modify the to field in the first request `POST /transfers` to the following
```
  "to": {
    "type": "CONSUMER",
    "idType": "ALIAS",
    "idValue": "abcdefghijklmnopqrstuvwxyz",
    "merchantClassificationCode": 123
  },
```
- Send post transfer call from payer pm4ml by executing the test case
