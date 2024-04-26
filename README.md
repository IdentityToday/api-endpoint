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
