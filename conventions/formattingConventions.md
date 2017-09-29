# Formatting Conventions #  

The iBanFirst Company Creation API conforms to the following general behavior for [RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer):

* You make [HTTPS](http://en.wikipedia.org/wiki/HTTPS) requests to the API endpoint, indicating the desired resources within the URL itself. (The public server, for the sake of security, only accepts [HTTPS](http://en.wikipedia.org/wiki/HTTPS) requests.)
* The [HTTP method](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) identifies what you are trying to do.  Generally, HTTP `GET` requests are used to retrieve information, while HTTP `POST` requests are used to make changes or submit information.
* If more complicated information needs to be sent, it will be included as [JSON-formatted](http://en.wikipedia.org/wiki/JSON) data within the body of the HTTP POST request.
  * This means that you must set `Content-Type: application/json` in the headers when sending POST requests with a body.
* Upon successful completion, the server returns an [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) of 200 OK, and a `Content-Type` value of `application/json`.  The body of the response will be a JSON-formatted object containing the information returned by the endpoint.

As an additional convention, all responses from iBanFirst-REST contain a `"success"` field with a boolean value indicating whether or not the success

* [Errors](#errors_conventions)
* [Normalized Types](#normalized_conventions)
	* [ID type](#type_id)
	* [Date type](#type_date)
	* [DateTime type](#type_datetime)
	* [Currency type](#type_currency)
	* [Phone type](#type_phone)
	* [QuotedDecimal type](#type_quoteddecimal)
* [Versioning](#versioning)

## <a id="errors_conventions"></a> Errors ##

iBanFirst uses conventional HTTP response codes to indicate success or failure of an API request. The body of the response contains more detailed information on the cause of the problem.

In general, the HTTP status code is indicative of where the problem occurred:

* Codes in the 200-299 range indicate success. 
    * Unless otherwise specified, methods are expected to return `200 OK` on success.
* Codes in the 400-499 range indicate that the request was invalid or incorrect somehow (e.g. a required parameter was missing, a trade failed, etc.). For example:
    * `400 Bad Request` occurs if the JSON body is malformed. This includes syntax errors as well as when invalid or mutually-exclusive options are selected.
    * `403  Forbidden` occurs if there is a problem with your authentication on the server.
    * `404 Not Found` occurs if the path specified does not exist, or does not support that method (for example, trying to POST to a URL that only serves GET requests)
* Codes in the 500-599 range indicate that the server experienced a problem. This could be due to a network outage or a bug in the software somewhere. For example:
    * `500 Internal Server Error` occurs when the server does not catch an error. This is always a bug. If you can reproduce the error, file it at [our bug tracker in your iBanFirst platform](https://iBanFirstplatform.com/login).
    * `502 Bad Gateway` occurs if iBanFirst-REST could not contact its `iBanFirst` server at all.
    * `504 Gateway Timeout` occurs if the `iBanFirst` server took too long to respond to the iBanFirst-REST server.

When possible, the server provides a JSON response body with more information about the error. These responses contain the following fields:

| Field | Type | Description |
|-------|------|-------------|
| errorCode | Integer | The code referring the error. |
| errorType | String | A short description identifying a general category for the error that occurred. |
| link | String | An hyperlink to access the page that describes more accurately the error. |

**Example:**

```js
"error": {
	"errorCode": 9,
	"errorType": "Entry already exists",
	"link": "http://fx4bix.com/RestError/OTFlNGY5ZWVlOTNjODZkZDVhZjg3YTlkNzBmMzgxZmI=/9"
}
```

Our API libraries can raise exceptions for many reasons, such as failed trade, invalid parameters, authentications errors, and network unavailability. We recommend always trying to gracefully handle exceptions from our API.

You can see a full list of errors and error details <a href="http://wonderfullmalus.fr/RestError/all/" target="_blank">here</a>.

## <a id="normalized_conventions"></a> Normalized Types ##

The iBanFirst API uses a set of types that describe a particular parameters and values.<br />
It allows developpers to know the format and the content to send for a particular field.

### <a id="type_id"></a> ID Type ###

The ID type describe an identifier for a certain object.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| ID | String | `^[A-Za-z0-9]{*}$` | A String representing the id of an object. This string contains alpha-numeric characters, including the capital ones. | `ND9dUV` |

### <a id="type_date"></a> Date Type ###

The Date type represents a date with its year, month and day.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Date | String | `^[0-9]{4}\-[0-9]{2}\-[0-9]{2}$` - `YYYY-MM-DD` | A String representing a date by its year, month and day in month. | `2015-07-14` |

### <a id="type_datetime"></a> DateTime Type ###

The Date type represents a date with its year, month and day, and also with hours, minutes and seconds.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| DateTime | String | `^((19[0,99]|2[0-9]{3})\-(0[1-9]|1[012])\-([012][0-9]|3[01])\ ([01][0-9]|2[0-3])\:([0-5][0-9])\:([0-5][0-9]))$` - `YYYY-MM-DD HH:mm:SS` | A String representing a date by its year, month, day in month, hour, minute and second. | `2015-07-14 10:55:37` |

### <a id="type_currency"></a> Currency Type ###

The Currency type represents a currency trigram.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Currency | String | `^[A-Z]{3}$` | A String representing the Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) of a currency. This String only contains capitalized letters. | `USD` |

### <a id="type_phone"></a> Phone Type ###

When a phone number is specified as part of a JSON body, it must be formated using [E.164](https://en.wikipedia.org/wiki/E.164) number formatting. This format is the internationally-standardized format for all phone numbers, and it includes all the relevant information to route calls and SMS messages globally. E.164 numbers can have a maximum of fifiteen digits and are usually written as follows [+][country code][subscriber number including area code]. 

A good reference to find a country's calling code is this Wikipedia page that [lists countries and their calling codes](https://en.wikipedia.org/wiki/List_of_country_calling_codes#Alphabetical_listing_by_country_or_region) .

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| phone | String | `^[A-Z]{6}$` | A String representing a phone number formated using  [E.164](https://en.wikipedia.org/wiki/E.164) number formatting. | `+33671738257` |

### <a id="type_quoteddecimal"></a> QuotedDecimal Type ###

The QuotedDecimal type describe an formatted floating number.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| QuotedDecimal | String | `^((\-){?})[0-9]{12}((\.[0-9]{1-7}){?})$` | A String representing a formatted floating number. | `"1.11723"` |

### <a id="type_nafCode"></a> Naf Code Type ###

The naf code type describe a formatted naf code.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| NafCode | String | `^[0-9]{4}[A-Z]{1}$` | A String representing a naf code. | `6209Z` | 

## <a id="versioning"></a> Versioning ##

When we make backwards-incompatible changes to the API, we realease new dated versions.
