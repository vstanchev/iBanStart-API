# Using webhooks #

The Webhooks provide realtime access to status update events of your iBanStart company creation. After configuring a destination URL, iBanStart will begin POSTing data directly to the URL when relevant activities occur.

Interacting with a third-party API like iBanStart can introduce a problem : Some events, like credit of accounts and many status event, are not the result of a direct API request.

Webhooks solve these problems by letting you register a URL that we will notified anytime an event happens in your iBanStart company creation. When an event occurs - for example, when a successful payment is allocated on a company account - iBanStart creates an 'event'. 
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

