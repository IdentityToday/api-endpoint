# API documentation for screening with IDToday

## Add association

This endpoint is used to add an association. It is an HTTP POST request to the specified URL.

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
  "businessId": "",
  "type": "PERSON",
  "name": "",
  "surname": "",
  "yearOfBirth": "",
  "country": ""
}
```

- ENTITY

```json
{
  "businessId": "",
  "type": "ENTITY",
  "registeredNameOfEntity": "",
  "country": ""
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

## Get association status

This endpoint makes an HTTP GET request to retrieve the status of a specific association. It requires the associationId as a query parameter in the URL.

### Request

#### Endpoint

```plaintext
GET /status-association

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

## Generate Screening Report

This API endpoint sends an HTTP GET request to retrieve a screening report for a specific association. The request should include the associationId as a query parameter in the URL. Upon successful execution, the API returns a JSON response with a downloadURL field that contains the URL for downloading the screening report. The screening report URL will only be returned if the association status is not `Potential Match`.

### Request

#### Endpoint

```plaintext
GET /generate-screening-report

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

## Add individual

This endpoint is used to add an individual. It is an HTTP POST request to the specified URL.

### Request

#### Endpoint

```plaintext
POST /add-individual

```

#### Request Body

##### Enums

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

###### `AddIndividualSchema`

This schema includes the following main sections:

- `businessId`: String
- `profileTypes`: Array of `Profile Types Enums`

###### `personalInformation`

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
- Various proofs and documents as base64 encoded files (optional)

###### `professionalInformation`

- `employer`: String (optional)
- `occupation`: String (optional)
- `positionAtCompany`: String (optional)
- `nameOfBusiness`: String (optional)
- `natureOfBusiness`: String (optional)
- `sourceOfIncome`: Array of `Source of Income Enums` (optional)
- `otherSourceOfIncome`: String (optional)
- `sourceOfFunds`: Array of `Source of Funds Enums` (optional)
- `otherSourceOfFunds`: String (optional)
- `addressOfBusiness`: String (optional)
- `businessContactNumber`: String (optional)
- `businessEmail`: String (email format, optional)
- `employmentStatus`: Enum (Employment Status Enums, optional)
- Various dates related to employment history (optional)

###### `BOFields`

- `dateBOBecameRegisterable`: Date (optional)
- `dateBOCeased`: Date (optional)

###### `marriageInformation`

- `spouseFullName`: String (optional)
- `spouseIdNumber`: String (optional)
- `spouseEmail`: String (email format, optional)
- `spouseMobileNumber`: String (optional)
- `marriageCertificate`: base64 encoded file (optional)
- `ancContract`: base64 encoded file (optional)
- `maritalStatus`: Enum (Marital Status Enums, optional)
- `maritalAgreement`: Enum (Marital Agreement Enums, optional)
- `spouseIdDocument`: base64 encoded file (optional)

###### `conveyancingFields`

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
   - If `hasPreviousSurname` is true, `previousSurname` is required.

2. **Financial Source Validation**:

   - For KYC and Conveyancing profiles:
     - If 'Other' is selected as a source of income, `otherSourceOfIncome` must be provided and not be empty.
     - If 'Other' is selected as a source of funds, `otherSourceOfFunds` must be provided and not be empty.

3. **Previous Surname Validation**:

   - If `hasPreviousSurname` is true, `previousSurname` is required.

4. **Conveyancing Validation**:
   - For conveyancing profiles:
     - `employmentStatus`, `maritalStatus`, and `transactionRole` are required.

Errors will throw a generic 'Data validation failed.' message if any condition is not met.

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
  "uid": ""
}
```

- The response will contain the following fields:

  - `uid` (string): unique identifier of the individual.

- Status: 400/401/500
- Content-Type: application/json

```json
{
  "error": "",
  "details": [
    {
      "code": "custom",
      "message": "Invalid base64 format for imageOfIdDocument",
      "path": ["personalInformation", "imageOfIdDocument"]
    }
  ]
}
```

- The response will contain the following fields:
  - `error` (string): message describing the error.
  - `details` (array || undefined): list of objects that describe validation errors.
