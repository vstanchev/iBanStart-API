# iBanStart API Documentation

## BEFORE YOU START ##

### API Reference ###

The iBanStart API is organized around REST. Our API is designed to have predictable, resource-oriented URLs and use HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support cross-origin resource sharing to allow you to interact securely with our API from a client-side web application. JSON will be returned in all responses from the API, including errors.

### Authentication ###

#### Quick ####

The API is designed to be secure. We highly recommend you to consult our [Authentication page](./services/Auth.md) before starting.

#### Time ####

The API authentication process is based on [Network Time Protocol](https://en.wikipedia.org/wiki/Network_Time_Protocol). The API engine has a tolerance of 2 seconds. Using the API with a non-synchronized system will result in various errors.

### Testing and troubleshooting ###

For your testing requests and to see possible API call responses, we invite you to visit our [testing page](./services/Testing.md) and use the provided script.

## GETTING STARTED WITH IBANSTART API ##

[Authentication](./services/Auth.md)  
[Testing](./services/Testing.md)  
[Endpoints](./services/API_compagny_creation.md)  
[Webhooks](./services/Webhooks.md)  
[Objects](./objects/objects.md)  
[Formatting Conventions](./conventions/formattingConventions.md) 

There are 4 simple steps to open a share capital account, transfer funds, get your share capital certificate, and release funds using iBanStart API:

#### 1. Create your company project ####

You need to use the following service to create your company project: [POST /companies/](./services/API_compagny_creation.md#post_company).

Mandatory company and shareholder documents and information will be required to initiate the process.

Document files must be sent using a specific API call: [POST /companies/{companyId}/Documents/{documentId}](./services/API_compagny_creation.md#post_documents_on_company).

The service will return an IBAN corresponding to the account where share capital should be deposited.

#### 2. Confirm that all funds have been received ####

You can use the following service to check the funds that have been received: [GET /Companies/{companyId}/](./services/API_compagny_creation.md#get_company).

This will start the review of your application. This proces can take up to 48 hours. The result is the generation of your share capital certificate.

#### 3. Download your share capital certificate ####

You will need to call the following service to check status of your application and retrieve your share capital certificate: [GET /Companies/{companyId}/CertificateOfDeposit/](./services/API_compagny_creation.md#get_certificateofdeposit_on_company).

#### 4. Release of share capital ####

Once you have received you have downloaded your share capital certificate, you will need to register your company at the Greffe using infogreffe.fr.

Once you have received your Kbis, you will need to upload it using the following service in order to release your share capital: [PUT /Companies/{companyId}/ReleaseDeposit/](./services/API_compagny_creation.md#put_releasedeposit_on_company)
 
#### 5. Know where you are in the process ####

You want to know if all shareholders has deposited funds to the IBAN? You can use the following API service: [GET /Companies/{companyId}/](./services/API_compagny_creation.md#get_company).
