---
 ## Detokenizer Service

---

{% include prerelease.html %}

Detokenizer Service allows clients to retrieve user's credit card number from Concur Expense in a secure way. Detokenizer service returns the card number encrypted with a public key that the client provides in the request. Client will be able to decrypt the card number using their private key.

* [Products and Editions](#products-editions)
* [Scope Usage](#scope-usage)
* [Access Token Usage](#access-token-usage)
* [Get credit card account details](#get-credit-card-account-details)


## <a name="products-editions"></a>Products and Editions

* Concur Expense Professional Edition

## <a name="scope-usage"></a>Scope Usage

Name|Description|Endpoint
---|---|---
`creditcardaccountnumber.read or creditcardaccount.read`|Reads credit card data from concur expense|/detokenizer/v1/company/{company}/creditcard/{creditcard} 

## <a name="access-token-usage"></a>Access Token Usage

This API requires a Company level JWT token

## <a name="get-credit-card-account-details"></a>Get credit card account details

Returns the credit card number encrypted with the public key provided in the request

### Scopes

`creditcardaccountnumber.read or creditcardaccount.read` - Refer to [Scope Usage](#scope-usage) for full details.

### Request

```shell
GET https://{region}.api.concursolutions.com/detokenizer/v1/company/{companyUUID}/creditcard/{creditcardGUID}
```

##### Parameters

Name|Type|Format|Description
---|---|---|---
`pubkeyAlgorithm `|`string`|query|RSA Algorithm used by PublicKey for card number encryption
`pubkeyFormat  `|`string`|query |Format used by Publickey
`pubkey  `|`string`|query|Public key
`company  `|`string`|path |Company UUID
`creditcard  `|`string`|path |Credit card GUID

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2) : Header used for authorization. Should be specified in the format 'Bearer JWT_Token'. This is a Company JWT token.

#### Payload

* None

### Response

#### Status Codes

* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)
* [404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)

#### Headers

* `concur-correlationid` is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
* [RFC 7230 Content-Length](https://tools.ietf.org/html/rfc7230#section-3.3.2)



#### Payload
Name|Type|Format|Description
---|---|---|---
`accountNumber  `|`string`|-|Encrypted credit card account number


### Example

#### Request

```shell
GET https://us.api.concursolutions.com/detokenizer/v1/company/d295261b-e624-442b-b771-9c7f00c1ed9a/creditcard/BB98EABE1DDA7B48B858CFDC7EEE2A10?pubkeyAlgorithm=rsa_pkcs7&pubkeyFormat=cert&pubkey=PUBLIC_KEY
Authorization: Bearer JWT_TOKEN
Accept: application/json
```

#### Response

```shell
HTTP/1.1 200 OK
concur-correlationid: 87de8598-dbd5-4aea-af9d-988efb61c468
Content-Type: application/json
Content-Length: 1270
```

```json
{
  "accountNumber": "MIAGCSqGSIb3DQEHA6CAMIACAQAxgbUwgbICAQAwGzAPMQ0wCwYDVQQDDARUZXN0AghyaD1Uj9uSsDANBgkqhkiG9w0BAQEFAASBgAk0/9Yd5CQt5/6vQ1gO9aSivBJrv4AOAluZ876tqVI+fCZi7P1YojC4nTkvl358zfD3vXE3ehj14FfIPZlwmuVlSZF4ad5ni2B78fs5Jr6lxhG9iPU0FyFv+NhuIet/mpEaaX2CWB8CUwkTVdDyT5UjrwqsvYpRCwLz0Hx76BO8MIAGCSqGSIb3DQEHATAdBglghkgBZQMEAQIEEPo3PO3VplgQ4mN0L5KInPKggAQgkqu7zWslGq3uqw0G2WXkK0QA2p0YHQuwhEPT2JMF5mUAAAAAAAAAA"
}
```

