# API Objects & Lists

* [Companies Object](#companies_object)
* [Company Creation Datas Object](#companyCreationDatas_object)
* [Shareholder Object](#shareholder_object)
* [Status List](#status_list)
* [Address Object](#address_object)
* [Account Object](#account_object)
* [Registered Individual Object](#registeredIndividual_object)
* [Registered Corporate Object](#registeredCorporate_object)
* [Amount Object](#amount_object)
* [Document Object](#document_object)
* [Document List](#document_list)

## Details ##

#### <a id="companies_object"></a> Companies Object ####

The main object in my company creation project. Status is automatically updated when there is a progress in my project.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the company to be created. |
| status | String (60) | The status of your company creation project. The full list of status is accessible in the [Status List](#status_list)  |
| companyCreationDatas | [Company Creation Datas Object](#companyCreationDatas_object) | Information, documents regarding the company you want to create. |
| shareholdingStructures | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on your company creation project. |
| account | [Account Object](#account_object) | The IBAN account that has been opened for the purpose of your company creation project. |

**Example:**
```js
"companies": {
    		"id": "NT4edA",
	 "status": "awaitingFunds",
	 "companyCreationDatas": {companyCreationDatas},
	 "shareholdingStructure": [{shareholder},{shareholder}],
	 "account": {account},
},
```
<hr />

#### <a id="companyCreationDatas_object"></a> Company Creation Datas Object ####

Specific information required for opening a company creation project.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String (100) | The legal name of the company to be created. |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| legalForm | String (5) | The legal form of the company to be created. It can be one of those 4 forms: `sasu`,`sarl`,`sas` and `eurl` |
| activityCode | [NAF ID](../conventions/formattingConventions.md#type_nafCode) | The code identifying the type of business of the company to be created. |
| shares | Integer | The number of shares to be issued. |
| sharesCapital | [Amount Object](#amount_object) | The amount in shareholding capital as mentionned in the articles of association. |
| percentageRelease | Integer (3) | The percentage of shareholding capital to be released when the company is created. "20", "50" or "100". |
| documents | Array<[Document Object](#document_object)> | The required documents for creating a company. Value for document object can take documentToComplete if your are posting a project and we will return documentCompleted when the document has been updated. |
| documentsToUpload | Array<[Document Object](#document_object)> | An Array of the document you need to upload with the [API Document Upload](#put_document). |

**Example:**

```js
"companyCreationDatas": {
    "registeredName": "Pied Pieper",
    "registeredAddress": {address},
    "legalForm":"sas",
    "activityCode":"6201Z",
    "authorizedCapital":{amount},
    "shares": 100000,
    "percentageRelease": 100,
    "documents": [{document},{document}]
	"documentsToUpload": [{documentsToUpload},{documentsToUpload}]

}
```

<hr />

#### <a id="shareholder_object"></a> Shareholder Object ####

This object shows the shareholder ownership and detailed information. 

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying in a unique way the shareholder. |
| type | String (10) | It can be only `Individual`. |
| isMainFounder | Boolean | Indicates who is introducing the project among the project. It can be `true` or `false`. You can only have on Main Founder. |
| isPep | Boolean | You indicates if the shareholder is legally recognized as a [PEP](https://en.wikipedia.org/wiki/Politically_exposed_person). `true` or `false`. |
| isFACTA | Boolean | You indicates if the shareholder is FACTA dependent, for more information, following the [link](https://fr.wikipedia.org/wiki/Foreign_Account_Tax_Compliance_Act) |
| fiscalNumber | Number | You must indicates a fiscal numver if the shareholder don't live in France | 
| shares | Integer | The number of shares that belong to the shareholder. |
| email | string (60) | Dedicated email of the shareholder. We may use this email to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have an email format. |
| registeredAddress | [Address Object](#address_object) | The registered address of the shareholder. |
| registeredIndividualName | [Registered Individual Object](#registeredIndividual_object) | The registered information of the shareholder when type is `individual`. |
| documents | Array<[Document Object](#document_object)> | The required documents related to this shareholder. |
| phoneNumber | [Phone](../conventions/formattingConventions.md#type_phone) | Dedicated phone number of the shareholder. We may use this number to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have the right format. |


**Example 1: individual shareholder**

```js
"shareholder": {
	"id": "XV4edA",
	"type": "individual",
	"isMainFounder": true,
	"isPep": false,
	"isFACTA": false,
    "fiscalNumber": null,
   	"shares": 50000,
    "phoneNumber": "+33999999999",
   	"registeredName": {registeredName},
	"email": "mch@ibanfirst.com",
	"registeredAddress": {registeredAddress},
	"registeredIndividualName": {registeredIndividualName},
	"documents": [{document},{document}],
	"documentsToUpload": [{documentsToUpload},{documentsToUpload}],
},
```

<hr />

#### <a id="registeredIndividual_object"></a> Registered Individual Object ####

Specific information when the shareholder is an individual.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| civility | String(3) | It can be `M` or `Mme`. |
| firstName | String(35) | The individual's first name. Truncated after the first 35 characters. |
| lastName | String(35) | The individual's last name. Truncated after the first 35 characters. |
| nationality | String (2) | The two-letters abbreviation for the country where the shareholder is registered if type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166).|
| birthDate | Date | The birth date of the shareholder when type is `individual`. `YYYY-MM-DD` |
| birthCity | String(35) | The indidual's birth city. Truncated after the first 35 characters. |
| birthCountry | String (2) | The two-letters abbreviation for the country where the shareholder is born when type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166). |
| profession | String | The profession you have |
| maritalStatus | String | It can be "Single","Married","Divorced","Widower" or "PACS" |



**Example:**

```js
"registeredIndividual": {
	"civility": "M",
	"firstName": "Maxime",
	"lastName": "Champoux",
	"nationality": "FR",
	"birthDate": 1991-06-25,
	"birthCity": "Pessac",
	"birthCountry": "FR",
	"profession": "CEO of another company",
	"maritalStatus": "Single"
},	
```
<hr />

#### <a id="status_list"></a> Status List ####

Here is the list of status you may encounter while using the iBanFirst API.

**Document resources:**

| Status | Description |
|-------|-------------|
| subscribing | Welcome to your company creation journey! A few document will be required to go to the next step and obtain you IBAN. |
| awaitingFunds | When your project is ready and all required document has been filled and signed, we return you this status together with you IBAN. |
| pendingKyc | When all funds has been collected and matched, you may ask for your certificate of deposit. This call triggered a KYC process on our side. |
| pendingInformation | While we are making our KYC process and reviewing your projects. We may ask you some more documents or information. Your project will not move to the enxt step until you provide the required document or information. |
| rejectedKyc | Something went wrong with your application, you are not compliant with our acceptance criterion. Please apply again or contact your account manager. |
| awaitingIncorporation | When certificate of deposit is available, you can use it to incorporate your company with the appropriate local authorities. |
| checkKbis | When certificate of deposit is available, you can use it to incorporate your company with the appropriate local authorities. |
| finalized | Here we are, you company is created and you capital has just been released to the bank account you just opened. Congrats! |

<hr />

#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account - will be an IBAN. |
| holderBank | String (11) | The code identifying iBanFirst or BIC/SWIFT code. |
| holder | String(50) | The recipient details, owner of the account. If the company is in pending creation, then [pending creation] will be added aside the owner name. |

**Example:**

```js
"account": {
    "currency": "EUR",
    "tag": "My payment account EUR",
    "accountNumber": "BE169816385163133",
    "holderBank": "FXBBBEBBXXX",
    "holderName": "Pied Pieper Paris [société en cours de formation]",
}
```

<hr />

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street | String(255) | The street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| state | String(2) | The state code for the address described. This field could be required if the country use a state system, like United States or Canada. To see a full list of state code, please refer to [this site](http://www.mapability.com/ei8ic/contest/states.php). |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "NY",
	"country": "US"
}
```

<hr />


#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | Float | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />

#### <a id="document_object"></a> Document Object ####

When an amount of document is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the document. |
| documentType | String (60) | The full list of document is accessible in the [Document List](#document_list)  |
| file | String | The binary content of the file, encoded with a base64 algorithm. |

**Example:**

```js
"document": {
    "id": "aB4edA",
    "documentType": "invoice",
    "file": "iVBORw0KGgoAAAANSU...Go2Z4OjsDO0sKl72GVORK5CYII=",
}
```

<hr />

#### <a id="documentToUpload_object"></a> Document to Upload Object ####

When an amount of documentToUpload is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the document. |
| documentType | String (60) | The full list of document is accessible in the [Document List](#document_list)  |
**Example:**

```js
"document": {
    "id": "aB4edA",
    "documentType": "invoice",
}
```

<hr />

#### <a id="document_list"></a> Document List ####

Here is the list of documents you may encounter while using the iBanFirst APi.

**Document resources for individuals:**

| Name | Description |
|-------|-------------|
| proofOfIdentity | An official document confirming your identity. |
| mandateShareholder | A mandate that deleguate powers of attorney from all shareholders to the main one : `mainShareholder`. |

**Document resources for corporates:**

| Name | Description |
|-------|-------------|
| articleOfAssociationDraft | Draft article of association as set-out in your company creation project. |
| articleOfAssociationSigned | Signed article of association when registered at the local authorities. |
| certificateOfIncorporation | Proof of incorporation of the company when registered at the local authorities. |
| certificateOfDeposit | Proof of fund deposit we deliver when receiving the full amount of expected capital. |
| openingAccountContract | A contract with our partner that delivers the certificate of deposit of funds. |
| finalOpeningAccountContract | A contract with iBanFirst to open an account once the company is created. |
| mandateShareholder | A mandate that deleguate powers of attorney from all shareholders to the main one : `mainShareholder`. |

<hr />






