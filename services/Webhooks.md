# Using webhooks #

The Webhooks provide realtime access to status update events of your iBanStart company creation. After configuring a destination URL, iBanStart will begin POSTing data directly to the URL when relevant activities occur.

Interacting with a third-party API like iBanStart can introduce a problem : Some events, like credit of accounts and many status event, are not the result of a direct API request.

Webhooks solve these problems by letting you register a URL that we will notified anytime an event happens in your iBanStart company creation.

When an event occurs - for example, when a successful payment is allocated on a company account - iBanStart creates an 'event'. 
This event contains all information about your project status. iBanStart then send the whole description of the iBanStart project to any URLs in your account's webhooks settings via an HTTP POST request.

Webhooks can be sent when :

* **Awaiting founders funds:** iBanStart is waiting for all shareholders to fund the company creation account.
* **Refused:** Company creation is refused.
* **Certificate of deposit:** The deposit certificate is available for download.
* **Awaiting KBIS:** Awaiting upload of the company KBIS.
* **KYC Suspend:** Mandatory documents are rejected and needs to be reuploaded.
* **Deposit of capital to confirm:** Awaiting confirmation of funds definitive locking.
* **KBIS Supsend:** The previously uploaded KBIS is rejected and needs to be reuploaded.

## 1. Configuring you webhook settings ##

To enable webhooks notification, you must send all relevant data to our institutional team in order to activate it.

You must provide an available URL able to receive POST HTTP or HTTPS requests with [application/json mime-type](https://en.wikipedia.org/wiki/JSON#MIME_type) support.

## 2. Receiving Webhook Events ##

Webhook data is sent as JSON in the POST request body. The full company creation details are included and can be used directly, after parsing the JSON into a [Company Object](../services/API_compagny_creation.md#company_object).

## 3. Responding to a webhook ##

To acknowledge receipt of a webhook, your endpoint should return a '200' HTTP status code. Any other information returned in the request header or request body is ignored. All response codes outside this range, including '3xx' codes will indicate to iBanFirst that you did not receive the webhook. This does mean that a URL redirection or a 'Not Modified' response will be treated as a failure.

If a webhook is not successfully received for any reason, iBanStart will continue trying to send the webhook with a certain delay :

* 1 minute for 3 times, then
* 10 minutes for 1 time, then 
* 30 minutes for 1 time, then 
* 1 hour for 1 time, then 
* every 4 hours

## Webhook body

The webhook body contains two main parts :

* The notification part where the notification data are included.
* The content part, where the [Company Object](../services/API_compagny_creation.md#company_object) is included.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| notificationId | [ID](../conventions/formattingConventions.md#type_id) | The unique identifier of the notification. You can use it for non-replay or support purposes. |
| firstSentDate | [DateTime](../conventions/formattingConventions.md#type_datetime) | The date and time for the first sending of the notification. |
| lastSentDate | [DateTime](../conventions/formattingConventions.md#type_datetime) | The date and time for the last sending of the notification. |
| notification | [Company Object](../services/API_compagny_creation.md#company_object) | The main content of the notification ; in this case, the company creation data. |

**Example:** 
```json
{
	"notificationId": "NTwD4N",
	"firstSentDate": "2017-09-28 21:00:00",
	"lastSentDate": "2017-09-28 22:00:00",
	"notification": {
	    "id": "NDgyMTc",
	    "companyData": {
	        "registredName": "Pied Pieper Paris",
	        "registredAddress": {
	            "street": "42 avenue de la grande arm\u00e9e",
	            "postCode": "75000",
	            "city": "Paris",
	            "country": "FR",
	            "state": null
	        },
	        "activityCode": "0119Z",
	        "legalForm": "SAS",
	        "capital": {
	            "amount": "100000",
	            "currency": "EUR"
	        },
	        "sharesNumber": "100",
	        "documents": [],
	        "documentsToUpload": [
	            {
	                "id": "NjQ4Mjk3",
	                "typeName": "CompagnyStatusDraft"
	            },
	            {
	                "id": "NjQ4Mjk4",
	                "typeName": "ContractFounderCreasoc"
	            }
	        ]
	    },
	    "status": "AwaitingCofounders",
	    "shareholdingStructures": [
	        {
	            "id": "ODY5Nzc",
	            "type": "Individual",
	            "isMainFounder": "0",
	            "isPep": "0",
	            "registeredName": {
	                "civility": "M",
	                "firstName": "Ruppe",
	                "lastName": "Arnaud",
	                "nationality": "FR",
	                "birthDate": "1991-01-01",
	                "birthCity": "Pessac",
	                "birthCountry": "FR",
	                "profession": "CEO of another company",
	                "maritalStatus": "Single"
	            },
	            "registeredCompagny": null,
	            "registeredIndividualCountry": null,
	            "phone": "+33999999999",
	            "email": "test@ibanfirst.com",
	            "numberOfParts": "50",
	            "regsiteredAddress": {
	                "street": "42 Test Avenue",
	                "postCode": "75008",
	                "city": "Paris",
	                "country": "FR",
	                "state": null
	            },
	            "documents": [],
	            "documentsToUpload": [
	                {
	                    "id": "NjQ4MzAx",
	                    "typeName": "Identity"
	                },
	                {
	                    "id": "NjQ4MzAy",
	                    "typeName": "ProofOfAddress"
	                }
	            ]
	        },
	        {
	            "id": "ODY5NzY",
	            "type": "Individual",
	            "isMainFounder": "1",
	            "isPep": "0",
	            "registeredName": {
	                "civility": "M",
	                "firstName": "Maxime",
	                "lastName": "Champoux",
	                "nationality": "FR",
	                "birthDate": "1991-01-01",
	                "birthCity": "Pessac",
	                "birthCountry": "FR",
	                "profession": "CEO of another company",
	                "maritalStatus": "Single"
	            },
	            "registeredCompagny": null,
	            "registeredIndividualCountry": null,
	            "phone": "+33999999999",
	            "email": "test@ibanfirst.com",
	            "numberOfParts": "50",
	            "regsiteredAddress": {
	                "street": "4 NEW YORK PLAZA, FLOOR 15",
	                "postCode": "75008",
	                "city": "Paris",
	                "country": "FR",
	                "state": null
	            },
	            "documents": [],
	            "documentsToUpload": [
	                {
	                    "id": "NjQ4Mjk5",
	                    "typeName": "Identity"
	                },
	                {
	                    "id": "NjQ4MzAw",
	                    "typeName": "ProofOfAddress"
	                }
	            ]
	        }
	    ],
	    "accounts": {
	        "iban": "FR7620033000010000003178307"
	    }
	}	
}
```