# API documentation for screening with IDToday

## Quick Navigation

- [Go to add association](#add-association)
- [Go to get association status](#get-association-status)
- [Go to generate screening report](#generate-screening-report)
- [Go to add individual](#add-individual)
- [Go to add entity](#add-entity)
- [Go to get submission status](#get-submission-status)
- [Go to generate compliance dashboard screening report](#generate-cd-screening-report)
- [Go to generate compliance dashboard kyc report](#generate-cd-kyc-report)

&nbsp;

# _Self Service Screening Portal Endpoints_

<a id="add-association"></a>

&nbsp;

## Add association

This endpoint is used to add an association. It is an HTTPS POST request to the specified URL.

### Request

#### Endpoint

```plaintext
POST /add-association

```

#### Request Body

Example Request Body:

- INDIVIDUAL

```json
{
  "businessId": "abc",
  "type": "PERSON",
  "name": "John",
  "surname": "Doe",
  "yearOfBirth": 1990,
  "country": "Namibia"
}
```

- ENTITY

```json
{
  "businessId": "abc",
  "type": "ENTITY",
  "registeredNameOfEntity": "Example Company",
  "country": "Namibia"
}
```

- The request should include the following parameters in the raw request body:
  - `businessId` (string): The ID of the business. _(Required)_
  - `type` (string): Type of the association. _(Required)_ `PERSON` | `ENTITY`
  - `name` (string): Name of the individual associated. _(Required if type = PERSON)_
  - `surname` (string): Surname of the individual associated. _(Required if type = PERSON)_
  - `registeredNameOfEntity` (string): Registered name of entity associated. _(Required if type = ENTITY)_
  - `yearOfBirth` (integer): Year of birth of the individual associated.
  - `country` (string): Nationality of the individual associated. _(Required)_

#### Response

- Status: 200
- Content-Type: application/json

```json
{
  "associationId": "",
  "caseId": "",
  "status": ""
}
```

- The response will contain the following fields:
  - `associationId` (string): ID of the association.
  - `caseId` (string): ID of the case.
  - `status` (string): Status of the association. `No Match` | `Potential Match` | `Match`

<a id="get-association-status"></a>

&nbsp;

## Get association status

This endpoint makes an HTTPS GET request to retrieve the status of a specific association. It requires the associationId as a query parameter in the URL.

### Request

#### Endpoint

```plaintext
GET /status-association?associationId={fc457f9b-bf2d-4245-b708-9dbaa7bda0ba}

```

#### Parameters

- associationId: (string) The unique identifier of the association.

### Response

- Status: 200
- Content-Type: application/json

```json
{
  "status": ""
}
```

The JSON object contains a status field, with one of the following values: `No Match` | `Potential Match` | `Match`

<a id="generate-screening-report"></a>

&nbsp;

## Generate Screening Report

This API endpoint sends an HTTPS GET request to retrieve a screening report for a specific association. The request should include the associationId as a query parameter in the URL. Upon successful execution, the API returns a JSON response with a downloadURL field that contains the URL for downloading the screening report. The screening report URL will only be returned if the association status is not `Potential Match`.

### Request

#### Endpoint

```plaintext
GET /generate-screening-report?associationId={fc457f9b-bf2d-4245-b708-9dbaa7bda0ba}

```

#### Query Parameters

- associationId: (string) The unique identifier of the association for which the screening report is requested.

### Response

- Status: 200
- Content-Type: application/json

```json
{
  "downloadURL": ""
}
```

The response contains a JSON object with a downloadURL field, which provides the URL for downloading the screening report.

&nbsp;

# _Compliance Dashboard Endpoints_

<a id="add-individual"></a>

&nbsp;

## Add individual

This endpoint is used to add an individual. It is an HTTPS POST request to the specified URL.

### Request

#### Endpoint

```plaintext
POST /add-individual

```

#### Data Structure

##### Enums## Enums and Types

###### Capital Sources Enums

- Salary
- Bonus
- Consultancy fees
- Allowance / Donation
- Inheritance
- Dividends / Profit share
- Board and sitting fees
- Rental income
- Investment income/returns
- Pension
- Sale of property
- Other

###### Profile Types Enums

- kyc
- bo
- conveyancing

###### Marital Status Enums

- Single
- Married
- Widow/er
- Divorced

###### Marital Agreement Enums

- Married in Community of Property
- Married out of Community of Property
- Married in terms of the laws of a foreign country
- Red-line Marriage

###### Transaction Role Enums

- Seller
- Purchaser

###### Payment Methods Enums

- Cash
- EFT
- Expected Foreign Transaction

###### Employment Status Enums

- Employed
- Self-Employed
- Unemployed
- Retired

##### Schema Structure

###### `AddIndividualSchema` (Object)

- `businessId`: String
- `profileTypes`: Array of `Profile Types Enums`

###### `personalInformation` (Object)

- `title`: String (optional)
- `firstNames`: String
- `surname`: String
- `dateOfBirth`: Date
- `nationality`: Enum (Country Enums)
- `countryOfBirth`: Enum (Country Enums, optional)
- `placeOfBirth`: String (optional)
- `hasPreviousSurname`: Boolean (optional)
- `previousSurname`: String (optional)
- `idNumber`: String (optional)
- `countryIssuingIdentification`: Enum (Country Enums, optional)
- `passportNumber`: String (optional)
- `passportExpiryDate`: String (optional)
- `foreignNationalIDNumber`: String (optional)
- `taxIdentificationNumber`: String (optional)
- `email`: String (email format, optional)
- `mobileNumber`: String (optional)
- `residentialAddress`: String (optional)
- `postalAddress`: String (optional)
- `alternativeEmail`: String (email format, optional)
- `homePhone`: String (optional)
- `proofOfResidence`: Base64 (optional)
- `imageOfIdDocument`: Base64 (optional)
- `imageOfPassport`: Base64 (optional)
- `proofOfFunds`: Base64 (optional)
- `signature`: Base64 (optional)
- `socialSecurity`: Base64 (optional)
- `bankConfirmationLetter`: Base64 (optional)
- `bankStatement`: Base64 (optional)
- `proofOfIncome`: Base64 (optional)
- `taxRegistrationCertificate`: Base64 (optional)
- `isPIP`: Boolean (optional)
- `PIPCapacity`: String (required if `isPIPEntity` is true)
- Various proofs and documents as base64 encoded files (optional)

###### `professionalInformation` (Object)

- `employer`: String (optional)
- `occupation`: String (optional)
- `positionAtCompany`: String (optional)
- `nameOfBusiness`: String (optional)
- `natureOfBusiness`: String (optional)
- `sourceOfIncome`: Array of `Capital Sources Enums` (optional)
- `otherSourceOfIncome`: String (optional)
- `sourceOfFunds`: Array of `Capital Sources Enums` (optional)
- `otherSourceOfFunds`: String (optional)
- `addressOfBusiness`: String (optional)
- `businessContactNumber`: String (optional)
- `businessEmail`: String (email format, optional)
- `employmentStatus`: Enum (Employment Status Enums, optional)
- Various dates related to employment history (optional)

###### `BOFields` (Object)

- `dateBOBecameRegisterable`: Date (optional)
- `dateBOCeased`: Date (optional)

###### `marriageInformation` (Object)

- `spouseFullName`: String (optional)
- `spouseIdNumber`: String (optional)
- `spouseEmail`: String (email format, optional)
- `spouseMobileNumber`: String (optional)
- `marriageCertificate`: base64 encoded file (optional)
- `ancContract`: base64 encoded file (optional)
- `maritalStatus`: Enum (Marital Status Enums, optional)
- `maritalAgreement`: Enum (Marital Agreement Enums, optional)
- `spouseIdDocument`: base64 encoded file (optional)

###### `conveyancingFields` (Object)

- `transactionRole`: Enum (Transaction Role Enums, optional)
- `bankName`: String (optional)
- `bankBranchCode`: String (optional)
- `bankAccountNumber`: String (optional)
- `bankAccountHolderName`: String (optional)
- `propertyAddress`: String (optional)
- `propertyRegisteredTo`: String (optional)
- `isThereBond`: Boolean (optional)
- `bondBankName`: String (optional)
- `bondAccountNumber`: String (optional)
- `purchaseFinancedWith`: String (optional)
- `paymentMethod`: Array of `Payment Methods Enums` (optional)
- `transactionFrequency`: String (optional)
- `transactionAmount`: String (optional)
- `conveyancingCountryInvolved`: Enum (Country Enums, optional)
- `partyRelationship`: String (optional)

###### Validation Logic

1. **Identity Document Validation**:

   - Namibian nationals must provide an ID number.
   - Non-Namibian nationals must provide a passport number.

2. **Financial Source Validation**:

   - For KYC and Conveyancing profiles:
     - If 'Other' is selected as a source of income, `otherSourceOfIncome` must be provided and not be empty.
     - If 'Other' is selected as a source of funds, `otherSourceOfFunds` must be provided and not be empty.

3. **Previous Surname Validation**:

   - If `hasPreviousSurname` is true, `previousSurname` is required.

4. **Conveyancing Validation**:
   - For conveyancing profiles:
     - `employmentStatus`, `maritalStatus`, and `transactionRole` are required.
     - Bond details (`bondAccountNumber` and `bondBankName`) will be required if `isThereBond` is true.
     - If `transactionRole` is `Purchaser` then `paymentMethod` and `purchaseFinancedWith` is required.
     - If `paymentMethod` includes `Expected Foreign Transaction` then `conveyancingCountryInvolved` is required.

## Usage

Example Request Body:

```json
{
  "businessId": "123",
  "profileTypes": ["kyc"],
  "personalInformation": {
    "title": "Mr",
    "firstNames": "John",
    "surname": "Doe",
    "dateOfBirth": "1990-01-01",
    "nationality": "Namibia",
    "idNumber": "12345678910"
  }
}
```

#### Response

- Status: 201
- Content-Type: application/json

```json
{
  "uid": "123-abc-987-zyx"
}
```

- The response will contain the following fields:

  - `uid` (string): unique identifier of the individual.

- Status: 400 / 401 / 500
- Content-Type: application/json

```json
{
  "error": "",
  "details": [
    {
      "code": "custom",
      "message": "File size exceeds 2MB limit for imageOfIdDocument. File size: 2.8MB",
      "path": ["personalInformation", "imageOfIdDocument"]
    }
  ]
}
```

- The response will contain the following fields:
  - `error` (string): message describing the error.
  - `details` (array OR undefined): list of objects that describe all validation errors. This will include the error code, message describing the error and the path.

<a id="add-entity"></a>

&nbsp;

## Add entity

This endpoint is used to add an entity. It is an HTTPS POST request to the specified URL.

### Request

#### Endpoint

```plaintext
POST /add-entity

```

#### Data Structure

##### Enums and Types

###### Profile Types Enums

- kyc
- conveyancing

###### Source of Income Enums

- Operating income
- Retained profits
- Sale of shares
- Sale of property
- Tax refund
- Licence fees
- Loan
- Capital/Equity raise
- Other

###### Source of Funds Enums

- Operating income
- Retained profits
- Sale of shares
- Sale of property
- Tax refund
- Licence fees
- Operating
- Other

###### Entity Type Enums

- Close Corporation
- Company
- Company (Foreign)
- Partnership
- Trust
- Association and other entities

###### Applicable Roles Enums

- CEO/Executive Manager
- Authorised Individual (can transact and/or establish business relationship on behalf of entity)
- Beneficial Owner

###### Transaction Role Enums

- Seller
- Purchaser

###### Payment Methods Enums

- Cash
- EFT
- Expected Foreign Transaction

###### Related Party Types

- Individual
- Company
- Trust
- Close Corporation
- Partnership
- Association and other entities

## Schema Definitions

### `ENTITY_GENERAL_INFORMATION`

Captures the general information of an entity:

- `registeredNameOfEntity`: String
- `tradingName`: String (optional)
- `countryOfRegistration`: Enum (Country Enums)
- `registrationNumber`: String (optional)
- `registeredAddress`: String (optional)
- `taxIdentificationNumber`: String (optional)
- `VATNumber`: String (optional)
- `email`: String (optional)
- `mobileNumber`: String (optional)
- `natureOfBusiness`: String (optional)
- `sourceOfFunds`: String (optional)
- `otherSourceOfFunds`: String (optional)
- `sourceOfIncome`: String (optional)
- `otherSourceOfIncome`: String (optional)
- `isOperatingAddressSameAsRegisteredAddress`: String (optional)
- `areThereMultipleOperatingAddresses`: String (optional)
- `operatingAddress`: String (optional)
- `headOfficeAddress`: String (optional)
- `tradeNameInCountryOfIncorporation`: String (optional)
- `tradeNameInNamibia`: String (optional)
- `nameOfManagementCompany`: String (optional)
- `productOrServiceRequired`: String (optional)

### `INDIVIDUAL_RELATED_PARTY`

Defines the schema for an individual related to the entity. This includes personal details and document-related fields that are crucial for identity verification and role assignment within the entity.

- `firstNames` (String)
- `surname` (String)
- `dateOfBirth` (Date)
- `hasPreviousSurname` (Boolean, optional)
- `previousSurname` (String, optional): Required if `hasPreviousSurname` is true.
- `nationality` (Enum)
- `idNumber` (String, optional): Required if `nationality` is "Namibia"
- `passportNumber` (String, optional): Required if `nationality` is NOT "Namibia"
- `passportExpiryDate` (String, optional): The expiry date of the passport, which must be provided if a passport number is given.
- `foreignNationalIDNumber` (String, optional)
- `idOrPassportDocument` (Base64, optional)
- `residentialAddress` (String, optional)
- `mobileNumber` (String, optional)
- `email` (Email, optional)
- `applicableRoles` (Array of Enums, optional): The roles applicable to the individual within the entity, such as CEO, Authorised Individual, or Beneficial Owner.
- `authorisationDocument` (Base64, optional)
- `executiveDocument` (Base64, optional)

#### `ENTITY_RELATED_PARTY`

Defines the schema for an entity that is related to another entity. This schema includes crucial details necessary for entity identification and verification within business transactions.

- `registeredNameOfEntity` (String)
- `countryOfRegistration` (Enum)
- `registrationDocument` (Base64, optional)
- `registrationNumber` (String)
- `registeredAddress` (String)
- `email` (Email, optional)
- `mobileNumber` (String, optional)

#### `MEMBER`

- Inherits all fields from `INDIVIDUAL_RELATED_PARTY` and includes:
  - `membershipPercentage` (Number)

#### `SHAREHOLDER`

- Inherits all fields from either `INDIVIDUAL_RELATED_PARTY` OR `ENTITY_RELATED_PARTY` and includes:
  - `numberOfShares` (Number)
  - `percentage` (Number)
  - `shareholderType` Enum (Related Party Types)

#### `PARTNER`

- Inherits all fields from either `INDIVIDUAL_RELATED_PARTY` OR `ENTITY_RELATED_PARTY` and includes:
  - `percentageInPartnership` (Number)
  - `typeOfPartner` Enum (Related Party Types)

#### `TRUST RELATED PARTY`

- Inherits all fields from either `INDIVIDUAL_RELATED_PARTY` OR `ENTITY_RELATED_PARTY` and includes:
  - `relatedPartyType` Enum (Related Party Types)

### Schemas for Different Entity Types

These schemas define the specific data requirements for various entity types such as Close Corporation, Company, Company (Foreign), Partnership, Trust, and Association and other entities. Each schema caters to the unique needs and regulatory requirements of these entity types.

#### Close Corporation Schema (Object)

- `members` (Array of `MEMBER`)
- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

#### Company Schema (Object)

- `directors` (Array of `INDIVIDUAL_RELATED_PARTY`)
- `shareholders` (Array of `SHAREHOLDER`, optional)
- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

#### Company (Foreign) Schema (Object)

- `directors` (Array of `INDIVIDUAL_RELATED_PARTY`)
- `shareholders` (Array of `SHAREHOLDER`, optional)
- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

#### Partnership Schema (Object)

- `partners` (Array of `PARTNER`)
- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

#### Trust Schema (Object)

- `trustees` (Array of `TRUST RELATED PARTY`)
- `beneficiaries` (Array of `TRUST RELATED PARTY`)
- `founder` (Array of `TRUST RELATED PARTY`)
- `typeOfTrust` (Enum): Specifies the type of trust, such as Inter vivos Trust, Testamentary Trust, or Foreign Trust.
- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

#### Associations and Other Entities Schema

- `otherRelatedParties` (Array of `INDIVIDUAL_RELATED_PARTY`, optional)

###### `conveyancingFields` (Object)

- `transactionRole`: Enum (Transaction Role Enums, optional)
- `bankName`: String (optional)
- `bankBranchCode`: String (optional)
- `bankAccountNumber`: String (optional)
- `bankAccountHolderName`: String (optional)
- `propertyAddress`: String (optional)
- `propertyRegisteredTo`: String (optional)
- `isThereBond`: Boolean (optional)
- `bondBankName`: String (optional)
- `bondAccountNumber`: String (optional)
- `purchaseFinancedWith`: String (optional)
- `paymentMethod`: Array of `Payment Methods Enums` (optional)
- `transactionFrequency`: String (optional)
- `transactionAmount`: String (optional)
- `conveyancingCountryInvolved`: Enum (Country Enums, optional)
- `partyRelationship`: String (optional)

### `AddEntitySchema`

This is the main schema for adding an entity:

- `businessId`: String
- `profileTypes`: Array of `Profile Types Enums`
- `entityType`: `ENTITY_TYPE_ENUMS`
- `entityGeneralInformation`: Nested `ENTITY_GENERAL_INFORMATION` with additional refinements
- `closeCorporation`, `company`, `companyForeign`, `partnership`, `trust`, `associationsAndOtherEntities`: Conditionally included based on `entityType`
- `documents`: Array of `DOCUMENT`
- `conveyancingFields`: Optional `CONVEYANCING_FIELDS`
- `isPIPEntity`: Boolean (optional)
- `PIPCapacity`: String (required if `isPIPEntity` is true)

## Validation Logic

The schema includes complex validations such as:

- Mandatory fields based on conditions (e.g., `operatingAddress` is required if `isOperatingAddressSameAsRegisteredAddress` is false).
- Refinements based on roles (e.g., `Executive Document` required for CEOs or executives).
- Entity-specific data requirements (e.g., `Trust` data required if `entityType` is `Trust`).
- Cross-field dependencies and checks (e.g., both `bondBankName` and `bondAccountNumber` must be provided if there is a bond).
- `conveyancingFields` will be required if `profileTypes` includes "conveyancing".
- Namibian nationals must provide an ID number.
- Non-Namibian nationals must provide a passport number.
- If 'Other' is selected as a source of income, `otherSourceOfIncome` must be provided and not be empty.
- If 'Other' is selected as a source of funds, `otherSourceOfFunds` must be provided and not be empty.
- If `isPIPEntity` is true then `PIPCapacity` is required.

## Usage

Example Request Body:

```json
{
  "businessId": "33",
  "profileTypes": ["kyc"],
  "entityType": "Partnership",
  "entityGeneralInformation": {
    "registeredNameOfEntity": "Partner & Partner",
    "countryOfRegistration": "Namibia"
  },
  "partnership": {
    "otherRelatedParties": [],
    "partners": [
      {
        "firstNames": "John",
        "surname": "Doe",
        "dateOfBirth": "1990-08-13",
        "nationality": "Namibia",
        "idNumber": "90081300663",
        "percentageInPartnership": 100,
        "typeOfPartner": "Individual"
      }
    ]
  }
}
```

#### Response

- Status: 201
- Content-Type: application/json

```json
{
  "uid": "123-abc-987-zyx"
}
```

- The response will contain the following fields:

  - `uid` (string): unique identifier of the entity.

- Status: 400 / 401 / 500
- Content-Type: application/json

```json
{
  "error": "",
  "details": [
    {
      "code": "custom",
      "message": "File size exceeds 2MB limit for imageOfIdDocument. File size: 2.8MB",
      "path": ["personalInformation", "imageOfIdDocument"]
    }
  ]
}
```

- The response will contain the following fields:
  - `error` (string): message describing the error.
  - `details` (array OR undefined): list of objects that describe all validation errors. This will include the error code, message describing the error and the path.

<a id="get-submission-status"></a>

&nbsp;

## Get submission status

This endpoint is used to get the KYC status of a submission. It is an HTTPS GET request to the specified URL.

### Request

#### Endpoint

```plaintext
GET /status-submission?submissionType={entities}&id={123}

```

#### Parameters

- submissionType: (string) Either `entities` | `individuals` | `trusts`.
- id: (string) If the submissionType is either `entities` or `trusts`, it should be the registration number. Otherwise if it's `individuals`, it should be their ID number (if Namibian) or passport number (if non-Namibian).

### Response

- Status: 200
- Content-Type: application/json

```json
{
  "status": "Pending"
}
```

The JSON object contains a status field, with one of the following values: `Pending` | `Complete`

<a id="generate-cd-screening-report"></a>

&nbsp;

## Generate Compliance Dashboard Screening Report

This endpoint is used to get the Screening report of a submission. It is an HTTPS GET request to the specified URL.

### Request

#### Endpoint

```plaintext
GET /generate-cd-screening-report?submissionType={entities}&id={123}

```

#### Parameters

- submissionType: (string) Either `entities` | `individuals` | `trusts`.
- id: (string) If the submissionType is either `entities` or `trusts`, it should be the registration number. Otherwise if it's `individuals`, it should be their ID number (if Namibian) or passport number (if non-Namibian).

### Response

- Status: 200
- Content-Type: application/json

```json
{
  "downloadURL": ""
}
```

The response contains a JSON object with a downloadURL field, which provides the URL for downloading the screening report from the Compliance Dashboard.

<a id="generate-cd-kyc-report"></a>

&nbsp;

## Generate Compliance Dashboard KYC Report

This endpoint is used to get the KYC report of a submission. It is an HTTPS GET request to the specified URL.

### Request

#### Endpoint

```plaintext
GET /generate-cd-kyc-report?submissionType={entities}&id={123}

```

#### Parameters

- submissionType: (string) Either `entities` | `individuals` | `trusts`.
- id: (string) If the submissionType is either `entities` or `trusts`, it should be the registration number. Otherwise if it's `individuals`, it should be their ID number (if Namibian) or passport number (if non-Namibian).

### Response

- Status: 200
- Content-Type: application/json

```json
{
  "downloadURL": ""
}
```

The response contains a JSON object with a downloadURL field, which provides the URL for downloading the KYC report from the Compliance Dashboard.
