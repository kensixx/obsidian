High-level Diagram - MS Point of View
![[UBGT_Microservice_High_Level_Diagram.excalidraw]]

## PlantUML Flow
```puml
participant ubo as "UB Online"
participant ms as "Microservice"

ubo->ubo: Select Country
ubo->ms: Get
```
## API Contract
Refer to `./ms-global-transfers/openapi` then run `npm start`  to view **Redocly / OpenAPI document for UBGT**
# ERD / ER Diagram

```puml

skinparam maxMessageSize 150

entity "countries" as countries {  
id: number  
---
name: varchar2(128)  
country_code: varchar2(128)  
status: varchar2(256)  
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "partners" as partners {  
id: number  
---
partner_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "preferred_partners" as preferred_partners {  
id: number  
---
partner_code: varchar2(128)  
country_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "currency_by_country" as currency_by_country {  
id: number  
---
name: varchar2(128)  
currency_list: varchar2(128)  
country_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "transaction_purpose" as transaction_purpose {  
id: number  
---
code: varchar2(128)  
description: varchar2(128)  
partner_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "transaction_source_of_funds" as transaction_source_of_funds {  
id: number  
---
code: varchar2(128)  
description: varchar2(128)  
partner_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "transaction_relationships" as transaction_relationships {  
id: number  
---
code: varchar2(128)  
description: varchar2(128)  
partner_code: varchar2(128)  
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "partner_links" as partner_links {  
id: number  
---
code: varchar2(128)  
description: varchar2(128)  
partner_code: varchar2(128)  
url: varchar2(128)
is_active: boolean
---
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

note right of partner_links::url
  This is exposed GRPC endpoint 
  of ms-nium / ms-dandelion 
  (or new Partner)
end note
```


Description
Description
Description
Description
Description
Description
Description
Description

country_code,currency_list
PH,"1,2,3,4,5,6,7,8"

# DynamoDB IAM Policy Permissions
Description: Just for reference, these were the issues we encountered when viewing the DynamoDB tables via Access + Secret Key
```
- dynamodb:ListTables
- dynamodb:DescribeTable
- dynamodb:Scan (for all tables)
- dynamodb:PartiQLSelect (for all tables) 
- `dynamodb:PartiQLInsert`
- `GetItem`
- `PutItem`
```

## DynamoDB - Sample Insert Data - PartiQL statement
```sql
INSERT INTO "transactions"
VALUE {
  'solId': 'solId',
  'channelId': 'apicChannelId',
  'date': '2023-09-12T17:08:16.038',
  'transaction_id': '09122023SWFTUBO12347',
  'sender': {
    'productId': 1,
    'channelUserId': 70195775,
    'accountNumber': 1234567,
    'firstName': 'Juan',
    'lastName': 'Dela Cruz',
    'lastName2': '',
    'middleName': 'Ponce',
    'suffix': 'Jr.',
    'nationality': 'Filipino',
    'postcode': 1158,
    'state': 'Metro Manila',
    'addressesType': '',
    'address': 'D Makita St.',
    'city': 'Makati City',
    'countryCode': 'PH',
    'countryOfBirth': 'PH',
    'accountType': 'INDIVIDUAL',
    'dob': '1992-12-20',
    'contactNumber': 9171234567,
    'identificationNumber': 'PASSPORT',
    'identificationType': 1234567,
    'issuedBy': 'Police',
    'issuedByCountry': 'PH',
    'identificationIssuedDate': '2017-03-08',
    'identificationExpirationDate': '2032-03-07',
    'bankAccountNumber': 1234567,
    'occupation': 'IT - Software Engineer',
    'incomeAmount': '30,000PHP',
    'sendingCorrespBranchNo': 123456,
    'payingCorrespSequenceId': 0,
    'bankId': 1326101,
    'bankBranchName': 'BankBranchName1',
    'bankBranchNo': 'BankBranchNo1',
    'bankBranchCity': 'Metropolitan Manila'
  },
  'beneficiary': {
    'accountNumber': 123456789,
    'accountNumberIBAN': 1234567,
    'addressesType': '',
    'address': 'Guadalupe Cebu City Cebu',
    'email': 'leni_robredo@gmail.com',
    'firstName': 'Leni',
    'lastName': 'Robredo',
    'middleName': 'Gerona',
    'currency': 'USD',
    'accountType': 'INDIVIDUAL',
    'countryCode': 'US',
    'paymentAccountType': null,
    'payoutMethod': 'NIUM',
    'bankAccountType': null,
    'postcode': 11580,
    'state': 'Los Angeles, California',
    'bankCode': null,
    'city': 'Compton',
    'contactCountryCode': null,
    'contactNumber': 917123567,
    'identificationNumber': 'PASSPORT',
    'identificationType': 1234567,
    'addressType': 'BILLING or MAILING',
    'valuetype': 'Low'
  },
  'remittance': {
    'type': 'SWIFT',
    'swiftCode': 'BOFAUS3NXXX',
    'currency': 'USD',
    'purpose': '5 630',
    'sourceOfFunds': 'A1000',
    'relationship': 1,
    'amount': 10,
    'chargeDetail': 'OUR',
    'customerComments': null
  },
  'tags': [
    'ADB_CLIENT',
    'HIGH_NETWORTH'
  ],
  'serviceFee': {
    'description': 'ADB AND HIGH NETWORTH PROMO V2',
    'breakdown': [
      {
        'description': 'Cable Charge',
        'creditAccountNumber': 2901015040012000,
        'value': 5
      }
    ]
  }
}
```