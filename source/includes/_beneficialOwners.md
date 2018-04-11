# Beneficial owners

Verified Customers of type `business` are required to verify the identity of beneficial owners in addition to the controller of the business if they satisfy the following requirements:

[bullet points here]

For more information on creating and verifying beneficial owners, reference our [Business Customer verification](https://developers.dwolla.com/resources/business-customer-verification.html) resource article.

### Beneficial owners resource

| Parameter | Description |
|-----------|------------|
| id | The beneficial owner unique identifier |
| firstName | The legal first name of the beneficial owner   |
| lastName |  The legal last name of the beneficial owner |
| status | Possible values of `verified`, `document`, or `incomplete`  |
| created | ISO-8601 timestamp. |

```noselect
{
  "firstName": "John",
  "lastName": "Doe",
  "status": "verified",
  "created": "2018-04-11T21:42:16.000Z"
}
```

## Create a beneficial owner

This section details how to create a new beneficial owner. To create `Beneficial Owners`, you need to collect the beneficial owner's full name, ssn, date of birth, and permanent address. Optionally, passport information must be included for foreign individuals that do not have a US issued ssn. `Beneficial Owners` require additional information that will give Dwolla the ability to confirm the identity of the individual.

### HTTP request
`POST https://api.dwolla.com/customers/{id}/beneficial-owners`

### Request Parameters

| Parameter | Required | Type | Description |
| ---------------|--------------|--------|----------------|
| firstName | yes  |  String |  The legal first name of the Beneficial Owner |
| lastName | yes | string | The legal last name of the Beneficial Owner |
| ssn | conditional | String | Full nine digits of Beneficial Owner’s social security number. |
| dateOfBirth | Yes | string | Beneficial owner’s date of birth in `YYYY-MM-DD` format. Must be 18 years or older. |
| address | Yes | object | Street number, street name of Beneficial Owner’s physical address |
| passport | conditional | object | An optional passport JSON object. Required for foreign individuals. Includes passport identification number and country.  |

### Address JSON object

| Parameter | Required | Type | Description |
|-----------|----------|----------------|-----------|
| address1 | yes | string | First line of the street address of the beneficial owner's permanent residence. **Note:** PO Boxes are not allowed. |
|  address2 | no | string | Second line of the street address of the beneficial owner's permanent residence. **Note:** PO Boxes are not allowed. |
| address3 | no | string | Second line of the street address of the beneficial owner's permanent residence. **Note:** PO Boxes are not allowed. |
| city | yes | string | City of beneficial owner's permanent residence. |
| stateProvinceRegion | yes | string | Two-letter US state or territory abbreviation code of Beneficial Owner’s physical address. For two-letter abbreviation reference, check out the [US Postal Service guide](https://pe.usps.com/text/pub28/28apb.htm). |
| country | yes | string | Country of beneficial owner's permanent residence. Two digit ISO code, e.g. `US`. |
| postalCode | yes | string | Postal code of beneficial owner's permanent residence. Should be a five digit postal code, e.g. `50314`. |

### Passport JSON object

| number | conditional | string | Required if Beneficial resides outside of United States and has no Social Security number. |
| country | conditional | string | Country of issued passport. |

### Request and response

```raw
POST https://api-sandbox.dwolla.com/customers/07d59716-ef22-4fe6-98e8-f3190233dfb8/beneficial-owners
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

{
  "firstName": "Jane",
  "lastName": "Doe",
  "ssn": "123-456-7890",
  "dateOfBirth": "1960-11-30",
  "address": {
    "address1": "123 Main St.",
    "city": "New York",
    "stateProvinceRegion": "NY",
    "country": "US",
    "postalCode": "10005"
  }
}

HTTP/1.1 201 Created
Location: https://api.dwolla.com/beneficial-owners/FC451A7A-AE30-4404-AB95-E3553FCD733F
```
```php
/**
 * No example for this language yet.
 **/
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C'
request_body = {
  :firstName => 'John',
  :lastName => 'Doe',
  :ssn => '123-456-7890',
  :dateOfBirth => '1970-01-01',
  :address => {
    :address1 => '99-99 33rd St',
    :city => 'Some City',
    :stateProvinceRegion => 'NY',
    :country => 'US',
    :postalCode => '11101'
  }
}

beneficial_owner = app_token.post "#{customer_url}/beneficial-owners", request_body
beneficial_owner.headers[:location] # => "https://api-sandbox.dwolla.com/beneficial-owners/AB443D36-3757-44C1-A1B4-29727FB3111C"
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
customer_url = 'https://api-sandbox.dwolla.com/customers/AB443D36-3757-44C1-A1B4-29727FB3111C'
request_body = {
  'firstName': 'John',
  'lastName': 'Doe',
  'dateOfBirth': '1970-01-01'
  'ssn': '123-456-7890'
  'address': {
    'address1': '99-99 33rd St',
    'city': 'Some City',
    'stateProvinceRegion': 'NY',
    'country': 'US'
    'postalCode': '11101'
  }
}

beneficial_owner = app_token.post('%s/beneficial-owners' % customer_url, request_body)
beneficial_owner.headers['location'] # => 'https://api-sandbox.dwolla.com/beneficial-owners/AB443D36-3757-44C1-A1B4-29727FB3111C'
```
```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/07d59716-ef22-4fe6-98e8-f3190233dfb8';
var requestBody = {
  firstName: 'John',
  lastName: 'Doe',
  dateOfBirth: '1970-01-01',
  ssn: '123-456-7890',
  address: {
    address1: '99-99 33rd St',
    city: 'Some City',
    stateProvinceRegion: 'NY',
    country: 'US'
    postalCode: '11101'
  }
};

appToken
  .post(`${customerUrl}/beneficial-owners`, requestBody)
  .then(res => res.headers.get('location')); // => 'https://api-sandbox.dwolla.com/beneficial-owners/FC451A7A-AE30-4404-AB95-E3553FCD733F'
```

## Retrieve a beneficial owner

This section contains information on how to retrieve a beneficial owner which belongs to a Customer.

### HTTP request
`GET https://api.dwolla.com/beneficial-owners/{id}`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Beneficial owner unique identifier. |

### Request and response

```raw
GET https://api-sandbox.dwolla.com/beneficial-owners/07D59716-EF22-4FE6-98E8-F3190233DFB8
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY


```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/07d59716-ef22-4fe6-98e8-f3190233dfb8'

beneficial_owner = app_token.get beneficial_owner_url
beneficial_owner.firstName # => "Jane"
```
```php
/**
 * No example for this language yet.
 **/
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/07d59716-ef22-4fe6-98e8-f3190233dfB8'

beneficial_owner = app_token.get(beneficial_owner_url)
beneficial_owner.body['firstName']
```
```javascript
var beneficialOwnerUrl = 'https://api-sandbox.dwolla.com/beneficial-owners/07d59716-ef22-4fe6-98e8-f3190233dfb8';

appToken
  .get(beneficialOwnerUrl)
  .then(res => res.body.firstName); // => 'Jane'
```


## List beneficial owners

This section contains information on how to retrieve a list of beneficial owners that belong to a Customer.

### HTTP request
`GET https://api.dwolla.com/customers/{id}/beneficial-owners`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Customer unique identifier. |

### Request and response

```raw
GET https://api-sandbox.dwolla.com/customers/176878b8-ecdb-469b-a82b-43ba5e8704b2/beneficial-owners
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

...

```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/176878b8-ecdb-469b-a82b-43ba5e8704b2'

beneficial_owners = token.get "#{customer_url}/beneficial-owners"
beneficial_owners._embedded.beneficial_owners[0].id # => "56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc"
```
```php
/**
 * No example for this language yet.
 **/
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
customer_url = 'https://api-sandbox.dwolla.com/customers/176878b8-ecdb-469b-a82b-43ba5e8704b2'

beneficial_owners = app_token.get('%s/beneficial-owners' % customer_url)
beneficial_owners.body['id']
```
```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/176878b8-ecdb-469b-a82b-43ba5e8704b2';

token
  .get(`${customerUrl}/beneficial-owners`)
  .then(res => res.body._embedded.beneficial_owners[0].id); // => '56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc'
```

## Remove a beneficial owner

Remove a beneficial owner id. A removed beneficial owner is soft deleted and can still be accessed when retrieved.

### HTTP request

`POST https://api.dwolla.com/beneficial-owners/{id}`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | id of beneficial owner to delete. |
| removed | yes | string |Specify a value of `true` to remove the associated beneficial owner. |

### HTTP status and error codes
| HTTP Status | Message |
|--------------|-------------|
| 404 | Beneficial owner not found. |

### Request and response

```raw
POST https://api-sandbox.dwolla.com/beneficial-owners/692486f8-29f6-4516-a6a5-c69fd2ce854c
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
{
    "removed": true
}

...

HTTP 200 OK
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/692486f8-29f6-4516-a6a5-c69fd2ce854c'

request_body = {
  :removed => true
}

app_token.post "#{beneficial_owner_url}", request_body
```
```php
/**
 * No example for this language yet.
 **/
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/692486f8-29f6-4516-a6a5-c69fd2ce854c'
request_body = {
  "removed": True
}

beneficial_owner = app_token.post(beneficial_owner_url, request_body)
```
```javascript
var beneficialOwnerUrl = 'https://api-sandbox.dwolla.com/beneficial-owners/692486f8-29f6-4516-a6a5-c69fd2ce854c';
var requestBody = {
  removed: true
};

appToken.post(beneficialOwnerUrl, requestBody);
```

## Retrieve beneficial ownership status

This section contains information on how to retrieve a Customer's beneficial ownership status.

### HTTP request
`GET https://api.dwolla.com/customers/{id}/beneficial-ownership`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Customer unique identifier. |

### Request and response

```raw
GET https://api-sandbox.dwolla.com/customers/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

...

```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/692486f8-29f6-4516-a6a5-c69fd2ce854c'

beneficial_ownership = app_token.get "#{customer_url}/beneficial-ownership"
beneficial_ownership.status # => "failed"
```
```php
/**
 * No example for this language yet.
 **/
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
customer_url = 'https://api-sandbox.dwolla.com/customers/692486f8-29f6-4516-a6a5-c69fd2ce854c'

beneficial_ownership = app_token.get('%s/beneficial-ownership' % customer_url)
beneficial_ownership.body['status'] # => 'failed'
```
```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/692486f8-29f6-4516-a6a5-c69fd2ce854c';

appToken
  .get(`${customer_url}/beneficial-ownership`)
  .then(res => res.body.status); // => "failed"
```

## Certify beneficial ownership

This section contains information on how to certify beneficial ownership for a business Verified Customer.

### HTTP request
`POST https://api.dwolla.com/customers/{id}/beneficial-ownership`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Customer unique identifier. |

### Request and response

```raw
POST https://api-sandbox.dwolla.com/customers/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc/beneficial-ownership
Accept: application/vnd.dwolla.v1.hal+json
Content-Type: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

{
  "status": "certified"
}
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
customer_url = 'https://api-sandbox.dwolla.com/customers/e52006c3-7560-4ff1-99d5-b0f3a6f4f909'
request_body = {
  :status => "certified"
}

app_token.post "#{customer_url}/beneficial-ownership", request_body
```
```javascript
var customerUrl = 'https://api-sandbox.dwolla.com/customers/e52006c3-7560-4ff1-99d5-b0f3a6f4f909';
var requestBody = {
  status: 'certified'
};

appToken.post(`${customerUrl}/beneficial-ownership`, requestBody);
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
customer_url = 'https://api-sandbox.dwolla.com/customers/e52006c3-7560-4ff1-99d5-b0f3a6f4f909'
request_body = {
    "status": "certified"
}

app_token.post('%s/beneficial-ownership' % customer_url, request_body)
```
```php
/**
 * No example for this language yet.
 **/
```

## Create a document for a beneficial owner

Create a document for a beneficial owner pending verification by uploading a photo of the document.  This requires a multipart form-data POST request.  The file must be either a `.jpg`, `.jpeg`, `.png`, `.tif`, or `.pdf` up to 10MB in size.

### HTTP request
`POST https://api.dwolla.com/beneficial-owners/{id}/documents`

### Request parameters
|Form Field| Description|
|----------|-------------|
| documentType | One of `passport`, `license`, `idCard`, or `other` |
| file | File contents. |

### Request and response

```raw
curl -X POST
\ -H "Authorization: Bearer tJlyMNW6e3QVbzHjeJ9JvAPsRglFjwnba4NdfCzsYJm7XbckcR"
\ -H "Accept: application/vnd.dwolla.v1.hal+json"
\ -H "Cache-Control: no-cache"
\ -H "Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW"
\ -F "documentType=passport"
\ -F "file=@foo.png"
\ 'https://api-sandbox.dwolla.com/beneficial-owners/1de32ec7-ff0b-4c0c-9f09-19629e6788ce/documents'

...

HTTP/1.1 201 Created
Location: https://api-sandbox.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0
```
```ruby
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/1DE32EC7-FF0B-4C0C-9F09-19629E6788CE'

file = Faraday::UploadIO.new('mclovin.jpg', 'image/jpeg')
document = app_token.post "#{beneficial_owner_url}/documents", file: file, documentType: 'license'
document.headers[:location] # => "https://api.dwolla.com/documents/fb919e0b-ffbe-4268-b1e2-947b44328a16"
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/1DE32EC7-FF0B-4C0C-9F09-19629E6788CE'

document = app_token.post('%s/documents' % customer_url, file = open('mclovin.jpg', 'rb'), documentType = 'license')
document.headers['location'] # => 'https://api-sandbox.dwolla.com/documents/fb919e0b-ffbe-4268-b1e2-947b44328a16'
```
```php
/**
 * No example for this language yet.
 **/
```
```javascript
var beneficialOwnerUrl = 'https://api-sandbox.dwolla.com/beneficial-owners/1DE32EC7-FF0B-4C0C-9F09-19629E6788CE';

var requestBody = new FormData();
body.append('file', fs.createReadStream('mclovin.jpg'), {
  filename: 'mclovin.jpg',
  contentType: 'image/jpeg',
  knownLength: fs.statSync('mclovin.jpg').size
});
body.append('documentType', 'license');

appToken
  .post(`${beneficialOwnerUrl}/documents`, requestBody)
  .then(res => res.headers.get('location')); // => "https://api-sandbox.dwolla.com/documents/fb919e0b-ffbe-4268-b1e2-947b44328a16"
```

## Retrieve a document for a beneficial owner

This section contains information on how to retrieve a document by its id.

### HTTP request
`GET https://api.dwolla.com/documents/{id}`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Document unique identifier. |

### Request and response

```raw
GET https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

...

{
  "_links": {
    "self": {
      "href": "https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc"
    }
  },
  "id": "56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc",
  "status": "pending",
  "type": "passport",
  "created": "2015-09-29T21:42:16.000Z"
}
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
document_url = 'https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc'

document = app_token.get document_url
document.type # => "passport"
```
```php
<?php
$documentUrl = 'https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc';

$documentsApi = new DwollaSwagger\DocumentsApi($apiClient);

$document = $documentsApi->getDocument($documentUrl);
$document->type; # => "passport"
?>
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
document_url = 'https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc'

documents = app_token.get(document_url)
documents.body['type'] # => 'passport'
```
```javascript
var documentUrl = 'https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc';

appToken
  .get(document_url)
  .then(res => res.body.type); // => "passport"
```
## List documents for beneficial owners

This section contains information on how to retrieve a list of documents that belong to a beneficial owner.

### HTTP request
`GET https://api.dwolla.com/beneficial-owners/{id}/documents`

### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| id | yes | string | Beneficial owner unique identifier. |

### Request and response

```raw
GET https://api-sandbox.dwolla.com/beneficial-owners/176878b8-ecdb-469b-a82b-43ba5e8704b2/documents
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

...

{
  "_links": {
    "self": {
      "href": "https://api-sandbox.dwolla.com/beneficial-owners/176878b8-ecdb-469b-a82b-43ba5e8704b2/documents"
    }
  },
  "_embedded": {
    "documents": [
      {
        "_links": {
          "self": {
            "href": "https://api-sandbox.dwolla.com/documents/56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc"
          }
        },
        "id": "56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc",
        "status": "pending",
        "type": "passport",
        "created": "2015-09-29T21:42:16.000Z"
      },
      {
        "_links": {
          "self": {
            "href": "https://api-sandbox.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0"
          }
        },
        "id": "11fe0bab-39bd-42ee-bb39-275afcc050d0",
        "status": "pending",
        "type": "passport",
        "created": "2015-09-29T21:45:37.000Z"
      }
    ]
  },
  "total": 2
}
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/176878b8-ecdb-469b-a82b-43ba5e8704b2'

documents = token.get "#{beneficial_owner_url}/documents"
documents._embedded.documents[0].id # => "56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc"
```
```php
/**
 * No example for this language yet.
 **/
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
beneficial_owner_url = 'https://api-sandbox.dwolla.com/beneficial-owners/176878b8-ecdb-469b-a82b-43ba5e8704b2'

documents = app_token.get('%s/documents' % beneficial_owner_url)
documents.body['total'] # => 2
```
```javascript
var beneficialOwnerUrl = 'https://api-sandbox.dwolla.com/beneficial-owners/176878b8-ecdb-469b-a82b-43ba5e8704b2';

token
  .get(`${beneficialOwnerUrl}/documents`)
  .then(res => res.body._embedded.documents[0].id); // => '56502f7a-fa59-4a2f-8579-0f8bc9d7b9cc'
```

* * *