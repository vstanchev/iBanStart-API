# iBanStart API Documentation

## BEFORE YOU START ##

### API Reference ###

The iBanFirst API is organized around REST. Our API is designed to have predictable, resource-oriented URLs and use the HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support cross-origin resource sharing to allow you to interact securely with our API from a client-side web application. JSON will be returned in all responses from the API, including errors.

### Authentication ###

#### Quick ####

As our API is designed to be secured, we highly recommend you to consult our [Authentication page](./services/Auth.md) before to start integration in order to get your application working properly with our services.

#### Time ####

As a part of the authentication process is based on time, your systems should be synchronised through [Network Time Protocol](https://en.wikipedia.org/wiki/Network_Time_Protocol) to ensure the capability of reaching our services. Our API engine has a tolerance of 2 seconds gap to ensure that all systems are capable to reach the API, whever the Time Server you uses. Using our API with non-synchronized system will result in various errors in response to your query. 

#### Testing and troubleshooting ####

For testing requests and have a clear representation of what's returned by our API, you may want visit our [testing page](./services/Testing.md) and use the script provided. Easy to use, it will help you integrate the API more efficiently. 

## GETTING STARTED WITH IBANSTART API ##

3 easy steps to get your company up!

#### 1. Create your company project ####

Please use the following service to create your company project: [POST /companies/](./services/API_compagny_creation.md#post_company).

This service will return an IBAN for the deposit of the 'Share Capital' of your future company.
We will require some mandatory documents and data to be completed in order to receive your company creation request.

PS: documents file must be completed using a specific API call : [POST /companies/{companyId}/Documents/{documentId}](./services/API_compagny_creation.md#post_documents_on_company).

When all transfers has been credited to the account, we will proceed to an analysis of your project. This latter can take up to 48 hours and will end with the generation of your certificate of deposit. In case you have implemented a webhook, you will be notified as soon as this certificate is available. In other cases, you may call our API to get the status and retrieve the document when available.

#### 2. Retrieve your certificate of deposit ####

You can use either this API service [GET /Companies/{companyId}/CertificateOfDeposit/](./services/API_compagny_creation.md#get_certificateofdeposit_on_company) to retrieve your certificate of deposit.

#### 3. Ask for your capital to be released  ####

You now have your certificate of incorporation, that's great! It means that your company now exists and that you will be able to open a Pro account with us and start your activity after a quick verification of your documents and data from our side.

You can use this API service to send us the last documents and data we will need from you: [PUT /Companies/{companyId}/ReleaseDeposit/](./services/API_compagny_creation.md#put_releasedeposit_on_company)
 
Using this call, you may also indicate where to release your capital. 

#### XX. Know where you are in your company creation project ####

You want to know if all shareholders has deposited funds to the IBAN? You can use this API service [GET /Companies/{companyId}/](./services/API_compagny_creation.md#get_company) or a taylor-made webhook to retrieve information and update the status of your project.
