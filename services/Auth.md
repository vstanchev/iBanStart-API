## Authentication ##

Every call on the IbanFirst API must be authenticated, which must be done by adding a custom HTTP header (X-WSSE) with the username and secret provided by the previous request. This section contains detailed instructions on how to create valid headers to execute requests on the API.

**Example of X-WSSE header:**

```js
X-WSSE: UsernameToken
	PasswordDigest="+QpstwYoZeToelOvVaObTdRdEZs=",
	Nonce="d36e3162829ed4c89851497a717f",
	Created="2014-03-20T12:51:45Z"
```
| Field | Description |
|-------|-------------|
| UsernameToken | Authentication method. The X-WSSE header must contain a UsernameToken as we only support token-based authentication. |
| PasswordDigest | Field containing the hashed token which will prove the authenticity of your request. It is essential that you recompute this hash for every request as a hash is only valid for a certain period, and then it expires. |
| Nonce | A random value used to make your request unique so it cannot be replicated by any other unknown party. |
| Created | Field containing the current UTC, GMT, ZULU timestamp (YYYY-MM-DDTHH:MM:SSZ) according to the ISO8601 format, *e.g. 2014-03-20T12:51:45Z*) |

**Computing the Password Digest** 

1.	Get a randomly generated 16 bytes Nonce formatted as 32 hexadecimal characters.
2.	Get the current Created timestamp in ISO-8601 format.
3.	Concatenate the following three values in this order: nonce, timestamp, secret.
4.	Calculate the SHA-1 hash value of the concatenated string, and make sure this value is in binary format! Some languages, like PHP, output hexadecimal hash by default. You may need to use special methods to obtain binary hashes in different languages or even convert byte to hex values by hand (see the sample codes below for more information).
5.	Apply a Base64 encoding to the resulted hash to get the final PasswordDigest value.

**Authenticate the third-party partner**

Albeit using the WSSE security to do request for a user to the API, the third-party partner must authenticate themselves. 

To authenticate, the third-party partner will be granted a partner token, a 16-character hexadecimal string, that will be mandatory to execute requests on the API. 

This partner token must be given by the “X-WSSE-REQUESTED-BY” HTTP header.

```js
X-WSSE-REQUESTED-BY: c6da61fcff03c20b
```

If you do not provide this header doing requests on the API, the request will be rejected.