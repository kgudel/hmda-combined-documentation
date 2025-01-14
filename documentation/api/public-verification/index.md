# HMDA Public Verification API

The following are HMDA Public API endpoints that do not require authentication. You may use these public endpoints to help prepare your data for sumbission.

## Single TS Verification

These endpoints are used to parse and run edits on a TS. Use `parse` to check for parsing errors and `validate/{{year}}` to check for syntactical, validity, and quality edits.

### Parse TS

This endpoint runs parsing on a Transmittal Sheet (TS). The endpoint returns a list of parsing errors in JSON format. If the TS is valid it the parsed TS object in JSON format.

  <b>Request:</b>

  ```
    curl -X POST \
    "https://ffiec.cfpb.gov/v2/public" \
    -H 'Content-Type: application/json' \
    -d '{
    "ts" : "1|Bank 0|2018|4|Jane|111-111-1111|user@bank.com|123 Main St|Washington|DC|20001|9|100|99-999999|10Bx939c5543TqA1144M"
  }'
  ```

<b>JSON Response:</b>

```
Method: POST
Endpoint: https://ffiec.cfpb.gov/v2/public
Payload: Pipe delimited TS
```

```json
{
    "id": 1,
    "institutionName": "Bank 0",
    "year": 2018,
    "quarter": 4,
    "contact": {
        "name": "Jane",
        "phone": "111-111-1111",
        "email": "user@bank.com",
        "address": {
            "street": "123 Main St",
            "city": "Washington",
            "state": "DC",
            "zipCode": "20001"
        }
    },
    "agency": 9,
    "totalLines": 100,
    "taxId": "99-999999",
    "LEI": "10Bx939c5543TqA1144M"
}
```

### Validate TS

This endpoint runs parsing and a specific year's edit checks on a Transmittal Sheet (TS). It returns edit details if the TS is able to be parsed or parsing errors if not.

_Note:_ Some edit checks require institution data. These checks are not run in any public APIs.

<b>Request:</b>

```
  curl -X POST \
  "https://ffiec.cfpb.gov/v2/public/ts/validate/2018" \
  -H 'Content-Type: application/json' \
  -d '{
  "ts" : "1|Bank 0|2018|4|Jane|111-111-1111|user@bank.com|123 Main St|Washington|DC|20001|9|100|99-999999|10Bx939c5543TqA1144M"
}'
```

<b>JSON Response:</b>

```
Method: POST
Endpoint: https://ffiec.cfpb.gov/v2/public/ts/validate/{{year}}
Payload: Pipe delimited TS
```

```json
{
    "syntactical": {
        "errors": []
    },
    "validity": {
        "errors": []
    },
    "quality": {
        "errors": []
    }
}
```

## Single LAR Verification

These endpoints are used to parse and and run edits on a LAR. Use `parse` to check for parsing errors and `validate/{{year}}` to check for syntactical, validity, and quality edits.

### Parse a LAR

This endpoint runs parsing on a single Loan Application Register (LAR). The endpoint returns a list of parsing errors in JSON format. If the LAR is valid it returns the parsed LAR object in JSON format.

<b>Request:</b>

```
  curl -X POST \
  "https://ffiec.cfpb.gov/v2/public/lar/parse" \
  -H 'Content-Type: application/json' \
  -d '{ "lar": "2|10Bx939c5543TqA1144M|10Bx939c5543TqA1144M999143X38|20180721|1|1|1|1|1|110500|1|20180721|123 Main St|Beverly Hills|CA|90210|06037|06037264000|1|1|1|1|1||1|1|1|1|1||3|3|5|7|7|7|7||||5|7|7|7|7||||3|3|1|1|3|3|30|30|36|1|0.428|1|1|750|750|1|9|1|9|10|10|10|10||2399.04|NA|NA|NA|NA|4.125|NA|42.95|80.05|360|NA|1|2|1|1|350500|1|1|5|NA|1|1|12345|1|1|1|1|1||1|1|1|1|1||1|1|1"
}'
```

 <b>JSON Response:</b>

 ```
Method: POST
Endpoint: https://ffiec.cfpb.gov/v2/public/lar/parse
Payload: Pipe delimited LAR
 ```

 ```json
  {
    "larIdentifier": {
        "id": 2,
        "LEI": "10BX939C5543TQA1144M",
        "NMLSRIdentifier": "12345"
    },
    "loan": {
        "ULI": "10Bx939c5543TqA1144M999143X38",
        "applicationDate": "20180721",
        "loanType": 1,
        "loanPurpose": 1,
        "constructionMethod": 1,
        "occupancy": 1,
        "amount": 110500.0,
        "loanTerm": "360",
        "rateSpread": "0.428",
        "interestRate": "4.125",
        "prepaymentPenaltyTerm": "NA",
        "debtToIncomeRatio": "42.95",
        "combinedLoanToValueRatio": "80.05",
        "introductoryRatePeriod": "NA"
    },
    "larAction": {
        "preapproval": 1,
        "actionTakenType": 1,
        "actionTakenDate": 20180721
    },
    "geography": {
        "street": "123 Main St",
        "city": "Beverly Hills",
        "state": "CA",
        "zipCode": "90210",
        "county": "06037",
        "tract": "06037264000"
    },
    "applicant": {
        "ethnicity": {
            "ethnicity1": 1,
            "ethnicity2": 1,
            "ethnicity3": 1,
            "ethnicity4": 1,
            "ethnicity5": 1,
            "otherHispanicOrLatino": "",
            "ethnicityObserved": 3
        },
        "race": {
            "race1": 5,
            "race2": 7,
            "race3": 7,
            "race4": 7,
            "race5": 7,
            "otherNativeRace": "",
            "otherAsianRace": "",
            "otherPacificIslanderRace": "",
            "raceObserved": 3
        },
        "sex": {
            "sex": 1,
            "sexObserved": 3
        },
        "age": 30,
        "creditScore": 750,
        "creditScoreType": 1,
        "otherCreditScoreModel": "9"
    },
    "coApplicant": {
        "ethnicity": {
            "ethnicity1": 1,
            "ethnicity2": 1,
            "ethnicity3": 1,
            "ethnicity4": 1,
            "ethnicity5": 1,
            "otherHispanicOrLatino": "",
            "ethnicityObserved": 3
        },
        "race": {
            "race1": 5,
            "race2": 7,
            "race3": 7,
            "race4": 7,
            "race5": 7,
            "otherNativeRace": "",
            "otherAsianRace": "",
            "otherPacificIslanderRace": "",
            "raceObserved": 3
        },
        "sex": {
            "sex": 1,
            "sexObserved": 3
        },
        "age": 30,
        "creditScore": 750,
        "creditScoreType": 1,
        "otherCreditScoreModel": "9"
    },
    "income": "36",
    "purchaserType": 1,
    "hoepaStatus": 1,
    "lienStatus": 1,
    "denial": {
        "denialReason1": 10,
        "denialReason2": 10,
        "denialReason3": 10,
        "denialReason4": 10,
        "otherDenialReason": ""
    },
    "loanDisclosure": {
        "totalLoanCosts": "2399.04",
        "totalPointsAndFees": "NA",
        "originationCharges": "NA",
        "discountPoints": "NA",
        "lenderCredits": "NA"
    },
    "nonAmortizingFeatures": {
        "balloonPayment": 1,
        "interestOnlyPayment": 2,
        "negativeAmortization": 1,
        "otherNonAmortizingFeatures": 1
    },
    "property": {
        "propertyValue": "350500.0",
        "manufacturedHomeSecuredProperty": 1,
        "manufacturedHomeLandPropertyInterest": 1,
        "totalUnits": 5,
        "multiFamilyAffordableUnits": "NA"
    },
    "applicationSubmission": 1,
    "payableToInstitution": 1,
    "AUS": {
        "aus1": 1,
        "aus2": 1,
        "aus3": 1,
        "aus4": 1,
        "aus5": 1,
        "otherAUS": ""
    },
    "ausResult": {
        "ausResult1": 1,
        "ausResult2": 1,
        "ausResult3": 1,
        "ausResult4": 1,
        "ausResult5": 1,
        "otherAusResult": ""
    },
    "reverseMortgage": 1,
    "lineOfCredit": 1,
    "businessOrCommercialPurpose": 1
}
```

### Validate a LAR

This endpoint runs parsing and a specific year's edit checks on a Loan Application Register (LAR). It returns edit details if the LAR is able to be parsed or parsing errors if not.

_Note:_ Some edit checks require institution data. These checks are not run in any public APIs. Macro edits are also not run this endpoint checks only a single LAR.

<b>Request:</b>

  ```
  curl -X POST \
  "https://ffiec.cfpb.gov/v2/public/lar/validate/2018" \
  -H 'Content-Type: application/json' \
  -d '{"lar": "2|B90YWS6AFX2LGWOXJ1LD|B90YWS6AFX2LGWOXJ1LD0KQ78F1Q02LVT4VCYK1G3D253|20180613|3|2|2|2|3|218910|5|20181031|1234 Hocus Potato Way|Tatertown|NM|14755|35003|35003976400|1|13||11||KE0NW|1|||||NA2IJJ7VBBQ15DTFRNK1PVWOPOXL3NH1PHUMN7S2J4|2|2|7|||||E120FYAU7BTSC3P51IL87C97W3N9VT791BMJI57RLJQSHOFDTUD7PQSPGHQ69D7I2P8JBDCBUIGRLX2BUS7SJR|DOOOI8UXY9PZDSRVFKP91CUQG95E88Y22KDR1AI3|1K1JINAYSIHCWBGJW3KOHU5D5TSK1Z61SUT5M9WQVWOHX|27|24|41|43|2|MS1LKLX7XZRKL23TV01I49RADZGUN0QY5AG9H4BJCVFTA4ZQ1EJUS1376QJXD87ZZDN5EFZIUWB8SK5EU34RVGOVTE|Y083OZN1VFT6B2XGL397ABL0Z4EV4CD45I7ZJ7FRSXXL4BRMKVPR5UCVV0K6IDLP7WLCBZAQ5KXT69PNE9PWQKCPKJB|UV0FTHG00G8WM65I7591IJYP9TEMXMDCVGZYRJTBUBBKEZI65HGL9ML|3|2|2|2|1|1|75|4|NA|0|NA|3|2|8888|8888|9||9||10|||||NA|NA|NA|NA|NA|NA|32|NA|NA|256|29|1111|1111|1111|1111|NA|3|2|16|Exempt|2|2|NA|3|1|5|1||DOREBESQSW1QT58SD2OZTHQUGXLSKCAJYZ63NJE2MUIAFQL4KW6PU26YSU786GT0IMCWWKCN25Y7KU0VLU0PPKWR8G6DKWI9BANPIE9I2ZZ5XDUX0TBAY4XFRFQZF087WS9ESTAKIV5V9HSZ2VXW7J5JMGPP4CGYA51BK68T57NN4KTKJVXIQMFXBTN5E3LGKKX3LITQ4C7OPFJ|7|6|5|5|||1111|2|1111"
}'
```

<b>JSON Response:</b>

```
Method: POST
Endpoint: https://ffiec.cfpb.gov/v2/public/lar/validate/{{year}}
Payload: Pipe delimited LAR
```

  ```json
{
  "syntactical": {
      "errors": []
  },
  "validity": {
      "errors": []
  },
  "quality": {
      "errors": [
          {
              "edit": "Q618",
              "description": "\"If Construction Method equals 2, then Manufactured Home Secured Property Type generally should not be 3.\""
          },
          {
              "edit": "Q631",
              "description": "\"If Loan Type equals 2, 3 or 4, then Total Units generally should be less than or equal to 4.\""
          },
          {
              "edit": "Q632",
              "description": "\"If Automated Underwriting System: 1; Automated Underwriting System: 2; Automated Underwriting System: 3; Automated Underwriting System: 4; or Automated Underwriting System: 5 equals 3, then the corresponding Automated Underwriting System Result: 1; Automated Underwriting System Result: 2; Automated Underwriting System Result: 3; Automated Underwriting System Result: 4; or Automated Underwriting System Result: 5 should equal 8 or 13.\""
          }
      ]
  }
}
```

## Full HMDA File Verification

The following endpoints can be used to parse and run edit checks on a full HMDA file.

### Parse a HMDA File

This endpoint runs parsing on a HMDA file. The endpoint returns a list of parsing errors in JSON format. If the file is valid it returns and empty list.

<b>Request:</b>

```
curl -X OPTIONS \
"https://ffiec.cfpb.gov/v2/public/hmda/parse" \
-F file=@<PATH>/<FILENAME>.txt
```

<b>JSON Response:</b>

```
Method: OPTIONS
URL: https://ffiec.cfpb.gov/v2/public/hmda/parse
Payload: HMDA file
```

```json
{
  "validated": []
}
```

### Validate a HMDA File

This endpoint runs parsing and a specific year's edit checks on a HMDA file. This endpoint runs checks row by row and so can return both parsing and edit check information if some rows pass parsing and others do not.

_Note:_ Some edit checks require institution data. These checks are not run in any public APIs. Macro edits are also not run as the check runs line by line.

<b>Request:</b>

```
curl -X POST \
"https://ffiec.cfpb.gov/v2/public/hmda/validate/2018" \
-F file=@<PATH>/<FILENAME>.txt
```

<b>JSON Response:</b>

```
Method: POST
Endpoint: https://ffiec.cfpb.gov/v2/public/hmda/validate/{{year}}
Payload: HMDA file
```

```json
{
  "parserErrors": [
      {
          "rowNumber": 1,
          "estimatedULI": "Transmittal Sheet",
          "errorMessages": [
              {
                  "fieldName": "Calendar Quarter",
                  "inputValue": "a",
                  "validValues": "Integer"
              }
          ]
      }
  ],
  "validationErrors": [
      [
          {
              "uli": "B90YWS6AFX2LGWOXJ1LDNIXOQ6OO7BRA5SLR6FSJJ5R89",
              "editName": "V619-2",
              "editDescription": "\"The Action Taken Date must be in the reporting year.\"",
              "fields": {
                  "Action Taken Date": "20180908",
                  "Application Date": "NA"
              }
          }
      ]
  ]
}
```