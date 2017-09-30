## API Routes ##

| Route | Description |
|-------|-------------|
| [POST /Companies/](#post_company) | Start your iBanStart project. You get all data for your company creation in return. |
| [GET /Companies/{comapnyId}/](#get_company) | Retrieve information and status about your company creation project |
| [POST /Companies/{comapnyId}/Documents/{documentId}](#post_documents_on_company) | Upload a document already declared in your project. |
| [GET /Companies/{comapnyId}/CertificateOfDeposit/](#get_certificateofdeposit_on_company) | Retrieve your certificate of deposit. |
| [PUT /Companies/{comapnyId}/ReleaseDeposit/](#put_releasedeposit_on_company) | Ask for your capital to be released. Let's make business now! |

## <a id="post_company"></a> Start your iBanStart project. ##

```
Method: POST 
URL: /Companies/
```
You want to create your company ? That's great! Let's start your project now, the following data is required to retrieve an IBAN:

The [New Company Object](../objects/objects.md#newCompany_object) that you will post will contain two important parts with required fields :

Your future company ([New Company Creation Data Object](../objects/objects.md#newCompanyCreationData_object)) with required :
* registeredName
* registeredAddress (street, postCode, city, country)
* legalForm
* activityCode
* sharesNumber
* sharesCapital (value, currency)
* percentageRelease

All founders ([New Shareholder Object](../objects/objects.md#newShareholder_object)) with required :
* type
* isMainFounder 
* sharesNumber
* email
* isPep
* registeredAddress (street, postCode, city, country)
* registeredIndividual (civility, firstName, lastName, nationality, birthDate, birthCity, birthCountry)

By submitting your project, you consider that your project is complete and all documents properly signed.
* We will return an IBAN that each shareholder can use to send their capital deposit. Status is `awaitingFunds` until all funds from shareholder has been correctly collected and matched.
* We will proceed a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated automatically to another status that means that your certificate of deposit has been generated and is ready to be retrieved.
* Next step will be to upload your kbis and ask for the release of the deposit.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| company | Body | [New Company Object](../objects/objects.md#newCompany_object) | Required | Data regarding your company creation project |

**Example of a Call containing all required information at this stage:**
```json
{
  "companyCreationData": {
    "registeredName": "Pied Pieper Paris",
    "registeredAddress": {
      "street": "42 avenue de la grande armée",
      "postCode": "75000",
      "city": "Paris",
      "country": "FR"
    },
    "activityCode": "0119Z",
    "legalForm": "SAS",
    "sharesCapital": {
      "value": 100000.00,
      "currency": "EUR"
    },
    "sharesNumber": 100,
    "percentageRelease": 50
  },
  "shareholdingStructure": [
    {
      "type": "Individual",
      "sharesNumber": 50,
      "isMainFounder": true,
      "isPep": false,
      "isFACTA": false,
      "fiscalNumber": null,
      "phone": "+33999999999",
      "email": "test@ibanfirst.com",
      "registeredAddress": {
        "street": "4 NEW YORK PLAZA, FLOOR 15",
        "postCode": "75008",
        "city": "Paris",
        "country": "FR"
      },
      "registeredIndividualName": {
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
    },
    {
      "type": "Individual",
      "sharesNumber": 50,
      "isMainFounder": false,
      "isPep": false,
      "isFACTA": false,
      "fiscalNumber": null,
      "phone": "+33999999999",
      "email": "test@ibanfirst.com",
	  "registeredAddress": {
        "street": "42 Test Avenue",
        "postCode": "75008",
        "city": "Paris",
        "country": "FR"
      },
      "registeredIndividualName": {
        "civility": "M",
        "firstName": "Ruppe",
        "lastName": "Arnaud",
        "nationality": "FR",
        "birthDate": "1991-01-01",
        "birthCity": "Pessac",
        "birthCountry": "FR",
		"profession": "CEO of another company",
		"maritalStatus": "Single"
      }
    }
  ]
}
```
**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| . | [Company Object](../objects/objects.md#company_object) | Your up-to-date company creation project description |

**Example:** 
```json
{
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
```
### <a id="get_companies"></a> Retrieve information related to a company creation project ###
```
Method: GET 
URL: /Companies/{comapnyId}/
```
You can use this API service to retrieve status and information on your company creation project.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| comapnyId | Path |[ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for your company creation project. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| . | [Company Object](../objects/objects.md#company_object) | Your up-to-date company creation project description |

**Example:** 
```json
{
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
```
### <a id="put_document"></a> Upload a document already declared in your project. ###
```
Method: POST
URL: /companies/{companyId}/document/{documentId}/
```
You have already declared documents that you must submit in you company creation project. In return we have sent you and ID for each of those documents.
You can upload those document one by one using this service and must use the ID we have sent you. This service allow you to upload document for the company creation project and 

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| companyId | Path | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| documentId | Path | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this document. |
| documentType | Body | String (60) | Required | The type of document to reference with your company creation project |
| tag | Body | String | Required | The name of the file you want to upload
| file | Body | String | Required | The binary content of the file, encoded with a base64 algorithm. |

**Example:**
```json
{
    "documentType": "Identity",
    "tag": "testIdentity.pdf",
    "file": "DPcCUEQs1hrqSxB3giGY620kQJ1BujlG4rOh+jjvEeW79GpT.....5Oj8dj1wQiKoqyaNGi4cOH51LYvn37k08+WVpaah4"
}
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| . | [Company Object](../objects/objects.md#company_object) | Your up-to-date company creation project description |

**Example:** 
```json
{
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
```
### <a id="getDocuments_certificateDeposit"></a> Retrieve your certificate of deposit ###

```
Method: GET 
URL: /companies/{companyId}/certificateOfDeposit/
```

You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.
This method return two files. It return the PDF file of the certificate of deposit and the audit file as proof of the legal signature

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| companyId | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for your company creation project. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| . | array of [Document To Download Object](../objects/objects.md#documentToDownload_object) | The array that contains the two documents you need to have a valid certificate deposit. |

**Example:** 
```json
[
    {
        "name": "1506588313_certificateDeposit.pdf",
        "mimeType": "application/pdf",
        "url": "http://172.21.40.43/app_dev.php/tl/52da48e3726d78cb983ca0f56dee1fca"
    },
    {
        "name": "1506588313_certificateDepositAuditTrail.pdf",
        "mimeType": "application/pdf",
        "url": "http://172.21.40.43/app_dev.php/tl/52da48e3726d78cb983ca0f56dee1fca"
    }
]
```
### <a id="put_companiesReleaseDeposit"></a> Ask for your capital to be released. Let's make business now. ###
```
Method: PUT
URL: /companies/{id}/releaseDeposit/
```
Congrats! At this stage, basically your company should be registered. You may use this service to automatically convert your company creation account into a Pro account. Otherwise, just send us the IBAN of the account you want to release your capital (it must be an account at your company's name). 

The following data will be required:
* accountNumber
* registrationNumber
* registrationDate
* Documents (articleOfAssociationSigned and proofOfIncorporation)

Our team will need a quick analysis of the documents you sent before you can enjoy our Pro services. The liberation of your funds must follow in the next 48 hours.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| registrationNumber | Body | String (20) | Required | The registration number of the company created. |
| registrationDate | Body | [Date](../conventions/formattingConventions.md#type_date) | Required | The registration date of the company created. |
| accountNumber | Body | String (20) | Optional | You may use this field if you don't want to release your capital in your iBanFirst Pro account. |
| documents | Body | Array of [New Document Object](../conventions/formattingConventions.md#newDocument_object) | Required | The type of document to reference with your company creation project |

**Example:**
```json
{
	"regitrationNumber": "814455614",
	"regitrationDate": "2017-06-25",
	"accountNumber": "FR76 3000 3009 9500 0503 4076 086",
	"documents": [
		{
			"documentType": "certificateOfIncoraporation",
			"tag": "certificateOfIncorporation.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAA...98FQAQqUAAAAASUVORK5CYII="
		},
		{
			"documentType": "articleOfAssociationSigned",
			"tag": "articleOfAssociationSigned.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAA...VORK5CYII="
		}
	]
}	
```
**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| . | [Company Object](../objects/objects.md#company_object) | Your up-to-date company creation project description |

**Example:** 
```json
{
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
```