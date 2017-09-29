# Using webhooks #

The Webhooks APIs provide realtime access to account data. After configuring a destination URL, iBanFirst will begin POSTing data directly to the URL when relevant activities occur.

Interacting with a third-party API like iBanFirst can introduce two problems: 
* Services not directly responsible for making an API request may still need to know the response of that request
* Some events, like credit of accounts and many status event, are not the result of a direct API request

Webhooks solve these problems by letting you register a URL that we will notify anytime an event happens in your account. When the event occurs - for example, when a successful payment is allocated on a company creation project, iBanFirst creates an 'event' object. This object contains all relevant information about what just happened, including the type of event and the data associated with that event. iBanFirst then send the 'event' object to any URLs in your account's webhooks settings via an HTTP POST request. You can find a full list of all event types in the [Types of events list](#events).

You might use webhooks as the basis to:
* Update a customer record in your database when a transfer is allocated to its account
* Email a customer when a transfer has failed to be allocated to a shareholder and has been rejected
* Update a customer record in your database when the status of its project is updated
* Email a customer when the 'certificate of deposit' has been provided
* Update a customer record in your database when the status of a document sent is updated
* Email a customent when a document provided is rejected and a new one must be uploaded again

## 1. Configuring you webhook settings ##

Webhooks are configured in the [Webhooks settings] section of the [API Supervisor Dashboard]. Clicking Add endpoint releals to add a new URL for receiving webhooks.

1. Create a web app with an endpoint to use as your webhook to receive events (e.g. https://mydomain.com/webhook/ibanfirst).
2. Make sure your webhook supports POST requests for incoming events and GET requests for XWSSE authentication
3. Register your webhook URL with your app using [`POST /webhooks/`](#post_webhooks)
You can enter any URL you'd like to have events sent to, but this should be a dedicated page on your server, coded per the instructions below. 
4. Use the returned 'webhookId' to sync your company with ['POST companies/-{id}/webhooks/-{id}/](#post_webhooksSubscription)

## 2. Receiving Webhook Events ##

Creating a webhook endpoint on your server is no different from creating any page on your website. With PHP, you might create a new .php file on your server; with a framework like symphony, you would add a new route with the desired URL.

Webhok data is sent as JSON in the POST request body. The full event details are included and can be used directly, after parsing the JSON into an [event object](#events).

## 3. Responding to a webhook ##

To acknowledge receipt of a webhook, your endpoint should return a '2xx' HTTP status code. Any other information returned in the request header or request body is ignored. All response codes outside this range, including '3xx' codes will indicate to iBanFirst that you did not receive the webhook. This does mean that a URL redirection or a 'Not Modified' response will be treated as a failure.

If a webhook is not successfully received for any reason, iBanFirst will continue trying to send the webhook once an hour for up to 3 days. Webhooks cannot be manually retried after this time, though you can [query for the event = TBD](#eventsObject) to reconcile your data with any missed events.

In the API supervisor interface, you can check how many times we've attempted to send an event to an endpoint by clicking on that endpoint URL in the Webhook details section. This will show you the latest response we received from you endpoint, along with a list of all attempted webhooks and the respective HTTP status codes we received.

<hr/>

# Routes and Lists #

## <a id="Routes"></a> API Routes ##

| Route | Description |
|-------|-------------|
| [`POST /webhooks/`](#post_webhooks) | Post your endpoint and get your webhook ID in return |
| [`POST /companies/-{id}/webhooks/-{id}/`](#post_webhooksSubscription) | Sync your webhook ID returned with an entity |

### <a id="post_webhooks"></a> Post your endpoint and get your webhook ID in return ###

```
Method: POST 
URL: /webhooks/
```

Use this service to register your webhook URL (endpoint) and get your webhook ID in return.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| endpoint | String | Required | Your webhook URL (endpoint). |

<hr />

### <a id="post_webhooksSubscription"></a> Sync your webhook ID returned with an entity ###

```
Method: POST 
URL: /companies/-{id}/webhooks/-{id}/
```

Use the returned 'webhookId' to sync your company with a specific endpoint.

<hr />

## <a id="events"></a> Lists of events ##

This is a list of all the types of events we currently send. we may add more at any time, so you shouldn't rely on only these types existing in your code.
You'll notice that these events follow a pattern: 'accountUpdate'. our goal is to design a consistent system that makes things easier to anticipate and code against. 

| event | Description |
|-------|-------------|
| [`entityCreated`](#entityUpdated) | Occurs whenever an new entity is created. |
| [`entityUpdated`](#entityUpdated) | Occurs whenever an entity status has changed. |
| [`transferCreated`](#transferCreated) | Occurs whenever a new transfer is created and allocated to your account. |
| [`transferReversed`](#transferReversed) | Occurs whenever a transfer is allocated to your account but has been rejected for compliance reason or upon your request. |
| [`transferUpdated`](#entityUpdated) | Occurs when a transfer has been allocated to your account and is now allocated to the right shareholder. |

## Transaction  webhooks ##

You can receive notifications via a webhook whenever there are new transactions associated with you company creation project.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|

```js
{
  "requestId": "ADDIOEH",
  "date": "2017-04-25 14:06:25",
  "notifications" [
     {
       "notificationId" :"SEFES21",
       “item”: “documents”, 
       "type": "documentUpdateStatus",
       "body": {
         “documentId”: “XXXXX”,
         “documentType”: “proofOfId”, 
         “Status”: “refused”,
         “error”: “Unreadable”,
       }
     },
     {
       "notificationId" :"SEFES22",
       “item”: “documents”, 
       "type": "documentRequired",
       "body": {
         “documentType”: “proofOfId”, 
         "companyId": "QEF455E"
       }
     }
     {
       "notificationId" :"SEFES23",
       “item”: "transaction", 
       "type": "transaction refused",
       "body": {
         "companyId": "QEF455E",
         "transaction":{
           "emetteur": ,
           "bankSource": ,
           "amount": ,
           "codeCommunication" : ,
           ...
         }
       }
     }
  ]
}
```
Les champs qui peuvent être présent l’objet transaction sont tous les champs qui sont disponible sur un mouvement.

## Item notification webhook ##

Webhooks are also used to communicate changes to an 'Item', such as an updated webhook, or errors encountered with an 'Item'. The error typically requires user action to resolve, such as when a document must be sent to.

```js
{
                "requestId": "XXXXX",
                "date": "2017-04-25 14:06:25",
                "notifications": [
                               { // document created notification (creasoc)
                                               "notificationId" :"XXXXX",
                                               "item": "documents", 
                                               "type": "documentRequired",
                                               "body": {
                                                               "companyId": "XXXXX"
                                                               "documentType": "proofOfId",
                                                               "on": "transaction",
                                                               "transactionId": "XXXX"
                                               }
                               }
                ]
}

```
