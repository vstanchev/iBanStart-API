# API Objects & Lists #

## Objects to be sent ##
[New Company Object](#newCompany_object)  
[New Company Creation Data Object](#newCompanyCreationData_object)  
[New Address Object](#newAddress_object)  
[New Amount Object](#newAmount_object)  
[New Shareholder Object](#newShareholder_object)  
[New Registered Individual Object](#newRegisteredIndividual_object)  
[New Document Object](#newDocument_object)  

## Object to be received ##
[Companies Object](#companies_object)  
[Company Creation Datas Object](#companyCreationDatas_object)  
[Shareholder Object](#shareholder_object)  
[Address Object](#address_object)  
[Account Object](#account_object)  
[Registered Individual Object](#registeredIndividual_object)  
[Amount Object](#amount_object)  
[Document Object](#document_object)  

## Object types ##
[Status List](#status_list)  
[Document List](#document_type_list)  

## Details ##

#### <a id="newCompany_object"></a> New Company Object ####

This object is meant to be posted when you create your iBanStart project. It contains all the required data to start the project.

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| companyCreationData 	| [New Company Creation Data Object](#newCompanyCreationDatas_object) | Required | Information, documents regarding the company you want to create. |
| shareholdingStructure | Array of [New Shareholder Object](#newShareholder_object) | Required | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on your company creation project. |

**Example:**
```json
{
	"companyCreationData": {NewCompanyCreationData},
	"shareholdingStructure": [
		{NewShareholder},
		{NewShareholder},
		...
	]
}
```

#### <a id="companyCreationData_object"></a> Company Creation Data Object ####

Specific information required for opening a iBanStart project.

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| registeredName | String (100) | Required | The legal name of the company to be created. |
| registeredAddress | [New Address Object](#newAddress_object) | Required | The registered address of the company to be created. |
| activityCode | [NAF ID](../conventions/formattingConventions.md#type_nafCode) | Required | The code identifying the type of business of the company to be created. |
| legalForm | String (5) | Required | The legal form of the company to be created. It can be one of those 4 forms: `sasu`,`sarl`,`sas` and `eurl` |
| sharesCapital | [New Amount Object](#newAmount_object) | Required | The amount in shareholding capital as mentionned in the articles of association. |
| sharesNumber | Integer | Required | The number of shares to be issued. |
| percentageRelease | Integer (3) | Required | The percentage of shareholding capital to be released when the company is created. "20", "50" or "100". |

**Example:**

```json
{
	"registeredName": "Pied Pieper Paris",
	"registeredAddress": {newAddress},
	"activityCode": "0119Z",
	"legalForm": "SAS",
	"sharesCapital": {NewAmount},
	"sharesNumber": 100,
	"percentageRelease": 50
}
```
#### <a id="newAddress_object"></a> New Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| street 	| String(255) 	| Required | The street for the address described. |
| postCode 	| String(15) 	| Required | The ZIP/Post code for the address described. |
| city 		| String(35) 	| Required | The city for the address described. |
| state 	| String(2) 	| Optional | The state code for the address described. This field could be required if the country use a state system, like United States or Canada. To see a full list of state code, please refer to [this site](http://www.mapability.com/ei8ic/contest/states.php). |
| country 	| String(2) 	| Required | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```json
{
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "NY",
	"country": "US"
}
```
#### <a id="newAmount_object"></a> New Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| value  | Float | Required | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | Required | The three-digit code specifying the currency related to the amount. |

**Example:**

```json
{
	"value": "10000.00",
	"currency": "GBP"
}
```
#### <a id="newShareholder_object"></a> New Shareholder Object ####

This object describes the shareholder ownership and detailed information to send along a iBanStart project creation. 

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| type | String (10) | Required | It can be only `Individual`. |
| sharesNumber | Integer | Required | The number of shares hold by this shareholder. |
| isMainFounder | Boolean | Required | Indicates who is introducing the project among the project. It can be `true` or `false`. You can only have on Main Founder. |
| isPep | Boolean | Required | You indicates if the shareholder is legally recognized as a [PEP](https://en.wikipedia.org/wiki/Politically_exposed_person). `true` or `false`. |
| isFACTA | Boolean | Required | You indicates if the shareholder is FACTA dependent, for more information, following the [link](https://fr.wikipedia.org/wiki/Foreign_Account_Tax_Compliance_Act) |
| fiscalNumber | Number | Required | You must indicates a fiscal numver if the shareholder don't live in France | 
| phone | [Phone](../conventions/formattingConventions.md#type_phone) | Required | Dedicated phone number of the shareholder. We may use this number to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have the right format. |
| email | string (60) | Required | Dedicated email of the shareholder. We may use this email to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have an email format. |
| registeredAddress | [New Address Object](#newAddress_object) | Required | The registered address of the shareholder. |
| registeredIndividualName | [New Registered Individual Object](#newRegisteredIndividual_object) | Optional | The registered information of the shareholder when type is `individual`. |

**Example:**

```json
{
	"type": "Individual",
	"sharesNumber": 50,
	"isMainFounder": true,
	"isPep": false,
	"isFACTA": false,
	"fiscalNumber": null,
	"phone": "+339999999999",
	"email": "test@ibanfirst.com",
	"registeredAddress": {NewAddress},
	"registeredIndividualName": {NewRegisteredIndividualName}
}
```
#### <a id="newRegisteredIndividual_object"></a> New Registered Individual Object ####

Specific information when the shareholder is an individual.

**Object resources:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| civility 		| String (3) 	| Required | It can be `Mr` or `Mrs`. |
| firstName 	| String (35) 	| Required | The individual's first name. Truncated after the first 35 characters. |
| lastName 		| String (35) 	| Required | The individual's last name. Truncated after the first 35 characters. |
| nationality 	| String (2) 	| Required | The two-letters abbreviation for the country where the shareholder is registered if type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166).|
| birthDate 	| [Date](../conventions/formattingConventions.md#type_date) 	| Required | The birth date of the shareholder when type is `individual`. `YYYY-MM-DD` |
| birthCity 	| String(35)	| Required | The indidual's birth city. Truncated after the first 35 characters. |
| birthCountry 	| String (2)	| Required | The two-letters abbreviation for the country where the shareholder is born when type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166). |
| profession 	| String (255)	| Required | The profession you have |
| maritalStatus | String 		| Required | It can be `Single`, `Married`, `Divorced`, or `PACS` |

**Example:**

```json
{
	"civility": "M",
	"firstName": "Maxime",
	"lastName": "Champoux",
	"nationality": "FR",
	"birthDate": "1991-01-01",
	"birthCity": "Pessac",
	"birthCountry": "FR",
	"profession": "CEO of another company",
	"maritalStatus": "Single"
}
```
#### <a id="newDocument_object"></a> Document Object ####

When a document is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| documentType | String (60) | The type of the document to upload. The full list of document is accessible in the [Document List](#document_list)  |
| tag | String | A custom name for the file to upload. |
| file | String | The binary content of the file, encoded with a base64 algorithm. |

**Example:**

```json
{
	"documentType": "proofOfIdentity",
	"tag": "Identity.pdf",
	"file": "..."
}
```
#### <a id="company_object"></a> Company Object ####

The main object of a iBanStart project. Status is automatically updated when there is a progress in the project.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The unique identifier related to the company to be created. |
| status | String (60) | The status of your company creation project. The full list of status is accessible in the [Status List](#status_list)  |
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Information, documents regarding the company you want to create. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on your company creation project. |
| account | [Account Object](#account_object) | The IBAN account that has been opened for the purpose of your company creation project. |

**Example:**
```json
{
    "id": "NDgyMTc",
    "status": "AwaitingCofounders",
    "companyData": {CompanyCreationData},
    "shareholdingStructures": [
    	{Shareholder},
    	{Shareholder}
    ],
    "account": {Account}
}
```
#### <a id="companyCreationData_object"></a> Company Creation Data Object ####

Specific information required for opening a company creation project.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String (100) | The legal name of the company to be created. |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| activityCode | [NAF ID](../conventions/formattingConventions.md#type_nafCode) | The code identifying the type of business of the company to be created. |
| legalForm | String (5) | The legal form of the company to be created. It can be one of those 4 forms: `sasu`,`sarl`,`sas` and `eurl` |
| capital | [Amount Object](#amount_object) | The amount in shareholding capital as mentionned in the articles of association. |
| sharesNumber | Integer | The number of shares to be issued. |
| documents | Array of [Document Object](#document_object) | The required documents for creating a company. Value for document object can take documentToComplete if your are posting a project and we will return documentCompleted when the document has been updated. |
| documentsToUpload | Array of [Document Object](#document_object) | An Array of the document you need to upload with the [API Document Upload](#put_document). |
| percentageRelease | Integer (3) | The percentage of shareholding capital to be released when the company is created. "20", "50" or "100". |

**Example:**

```json
{
	"registredName": "Pied Pieper Paris",
	"registredAddress": {Address},
	"activityCode": "0119Z",
	"legalForm": "SAS",
	"capital": {Amount},
	"sharesNumber": "100",
	"documents": [],
	"documentsToUpload": [
		{Document},
		{Document},
		...
	]
}
```
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


**Example:**

```json
{
	"id": "ODY5Nzc",
	"type": "Individual",
	"isMainFounder": "0",
	"isPep": "0",
	"registeredName": {RegisteredIndividual},
	"registeredCompagny": null,
	"registeredIndividualCountry": null,
	"phone": "+33999999999",
	"email": "test@ibanfirst.com",
	"numberOfParts": "50",
	"regsiteredAddress": {Address},
	"documents": [],
	"documentsToUpload": [
		{Document},
		{Document},
		...
	]
}
```
#### <a id="registeredIndividual_object"></a> Registered Individual Object ####

Specific information when the shareholder is an individual.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| civility 		| String (3)  | It can be `Mr` or `Mrs`. |
| firstName 	| String (35)  | The individual's first name. Truncated after the first 35 characters. |
| lastName 		| String (35)  | The individual's last name. Truncated after the first 35 characters. |
| nationality 	| String (2)  | The two-letters abbreviation for the country where the shareholder is registered if type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166).|
| birthDate 	| [Date](../conventions/formattingConventions.md#type_date) | The birth date of the shareholder when type is `individual`. `YYYY-MM-DD` |
| birthCity 	| String(35) | The indidual's birth city. Truncated after the first 35 characters. |
| birthCountry 	| String (2) | The two-letters abbreviation for the country where the shareholder is born when type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166). |
| profession 	| String (255) | The profession you have |
| maritalStatus | String | It can be `Single`, `Married`, `Divorced`, or `PACS` |

**Example:**

```json
{
	"civility": "M",
	"firstName": "Maxime",
	"lastName": "Champoux",
	"nationality": "FR",
	"birthDate": "1991-01-01",
	"birthCity": "Pessac",
	"birthCountry": "FR",
	"profession": "CEO of another company",
	"maritalStatus": "Single"
}
```
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

```json
{
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "75008",
	"city": "Paris",
	"state": null
	"country": "FR"
}
```
#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| iban | string(27) | The [IBAN](https://en.wikipedia.org/wiki/International_Bank_Account_Number) of the account. |

**Example:**

```json
{
    "iban": "FR7620033000010000003178307"
}
```
#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | Float | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```json
{
	"value": "10000.00",
	"currency": "GBP"
}
```
#### <a id="document_object"></a> Document Object ####

When an amount of documentToUpload is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the document. |
| documentType | String (60) | The full list of document is accessible in the [Document List](#document_list)  |

**Example:**

```json
{
    "id": "aB4edA",
    "documentType": "invoice"
}
```
#### <a id="documentToDownload_object"></a> Document To Download Object ####

When an amount of Document To Download is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| name | string(255) | The name of the document to download. |
| mimeType | String (60) | The MIME-type of the document to download. |
| url | String (255) | The URL to download the document. Note that this URL is unique, and cannot be called twice. If you fail the download the first time, you must recall this request to generate new URLs. |

**Example:**

```json
{
	"name": "1506588313_certificateDeposit.pdf",
	"mimeType": "application/pdf",
	"url": "http://api.ibanfirst.com/tl/52da48e3726d78cb983ca0f56dee1fca"
}
```

#### <a id="status_list"></a> Status List ####

Here is the list of status you may encounter while using the iBanFirst API.

**Document resources:**

| Status | Description |
|-------|-------------|
| Awaiting founders funds                       | Awaiting for all shareholder to fund the company account. |
| Awaiting iBanFirst KYC                        | IbanFirst executes a KYC procedure on the company. |
| Refused                                       | The company creation have been refused. |
| Partner KYC/KYT                               | A partner executes a KYC/KYT procedure on the company |
| Reject by partner                             | The KYC or KYT rejected by the partner. |
| Deposit of funds                              | All funds have been sent by the shareholders. |
| Certificate of deposit                        | The deposit certificate is available for download. |
| Awaiting KBIS                                 | Awaiting the company KBIS. |
| Check KBIS                                    | The KBIS is validated. |
| Kbis refused                                  | The KBIS has been rejected. |
| Certificate of release                        | The release certificate is created. |
| Release of funds                              | The funds of the company are beeing released. |
| Funds released                                | The funds of the company are released. |
| KYC Suspend                                   | The KYC procedure is suspended. |
| Closed                                        | The company creation is finalized. |
| Waiting Generation of Certificate of deposit  | Awaiting deposit certificate generation. |
| KYC partner compliance                        | The bank partner is running a compliance KYC procedure on the company. |
| Deposit of capital to confirm                 | Awaiting the confirmation for the capital locking. |
| Fund send to BP                               | The company funds are sent to the bank partner. |
| BP Wait for Fund                              | The bank partner is waiting for the funds to arrive. |
| Awaiting KYC MOManager                        | Awaiting middle office to execute the KYC procedure. |
| Awaiting iBanFirst KYC Compliance             | Awaiting compiance office to execute the KYC procedure. |
| Deposit of capital to confirm by MO           | Awaiting the capital deposit from the middle office. |

#### <a id="document_type_list"></a> Document List ####

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