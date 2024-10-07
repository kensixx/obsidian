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
**Description:** Request body of UBO to send to APIC -> Native MS

TODO: Swagger Doc of this

Notes:
- Date and Time - 
```json
{
  "solId": "solId",
  "channelId": "apicChannelId",
  "date": "2023-09-12",
  "time": "HH:MM:SS",
  "referenceId": "09122023SWFTUBO12347",
  "sender": {
    "productID": 1,
    "channelUserId": 70195775,
    "accountNumber": "accountNumber",
    "firstName": "",
    "lastName": "",
    "middleName": "",
    "suffix": "",
    "nationality": "",
    "postcode": "11580",
    "state": "",
    "address": "D Makita St.",
    "city": "New Jersey",
    "countryCode": "AE",
    "accountType": "INDIVIDUAL",
    "dob": "1992-12-20",
    "identificationNumber": "PASSPORT",
    "identificationType": "1234567"
  },
  "beneficiary": {
    "accountNumber": "123456789",
    "addressesType": "",
    "address": "Guadalupe, Cebu City, Cebu",
    "email": "leni_robredo@gmail.com",
    "firstName": "",
    "lastName": "",
    "middleName": "",
    "currency": "",
    "accountType": "INDIVIDUAL",
    "countryCode": "AE",
    "paymentAccountType": "",
    "payoutMethod": ""
  },
  "remittance": {
    "type": "SWIFT",
    "swiftCode": "BOFAUS3NXXX",
    "currency": "USD",
    "purpose": "5 630",
    "sourceOfFunds": "A1000",
    "relationship": "1",
    "amount": 10,
    "chargeDetail": "OUR",
    "customerComments": ""
  },
  "tags": [
    "ADB_CLIENT",
    "HIGH_NETWORTH"
  ],
  "serviceFee": {
    "description": "ADB AND HIGH NETWORTH PROMO V2",
    "breakdown": [
      {
        "description": "Cable Charge",
        "creditAccountNumber": "2901015040012000",
        "value": 5
      },
      {
        "description": "Bank Commission",
        "creditAccountNumber": "2901015040012000",
        "value": 5
      }
    ]
  }
}
```


# ERD / ER Diagram

```puml

skinparam maxMessageSize 150

entity "countries" as countries {  
id: number  
---
name: varchar2(128)  
country_code: varchar2(128)  
status: varchar2(256)  
created_date: varchar2(128)  
modified_date: varchar2(10)  
modified_by: varchar2(10)  
}

entity "partners" as partners {  
id: number  
---
partner_code: varchar2(128)  
is_active: boolean
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