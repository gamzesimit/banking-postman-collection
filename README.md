# Banking Postman collection

[![tests](https://github.com/gamzesimit/banking-postman-collection/actions/workflows/tests.yml/badge.svg)](https://github.com/gamzesimit/banking-postman-collection/actions/workflows/tests.yml)

A Postman collection for a retail banking API, with the assertions written into
the requests and the whole thing run headless by Newman on every commit.

Seven requests, seventeen assertions.

## Running it

In Postman: import `collection/parabank.postman_collection.json` and
`environments/local.postman_environment.json`, pick the environment, run the
collection.

From the command line:

```bash
docker compose up -d
npm install -g newman
newman run collection/parabank.postman_collection.json \
  -e environments/local.postman_environment.json
```

## How the requests are grouped

By what they prove, not by endpoint.

| Folder | What it proves |
|---|---|
| Accounts | The shape of an account, that money carries two decimal places, that an unknown account is not reported as success, and that every account in a list belongs to the customer asked for |
| Transfers | A valid transfer succeeds inside its time budget, the paying account falls by exactly the amount, and a transfer to an account that does not exist is refused |
| Customers | A customer record carries a name and does not carry a password |

## Why the balance is checked as a difference

The collection reads the balance before the transfer, stores it, then reads it
again afterwards and asserts on the difference. Asserting on an absolute figure
would mean the collection could only be run once against a given environment,
and a collection that cannot be re-run is a collection nobody runs.

## What is deliberately not here

Negative and zero amounts. They are covered in
[banking-api-tests](https://github.com/gamzesimit/banking-api-tests) where the
defects they expose are written up properly. Repeating them here would mean two
places to update when one of them is fixed.
