# FriiPay Developer Notes
Developer guidance and proposed APIs for integration with the Frii Pay crypto payment platform.
## Overview
This document outlines the proposed services offered by the Frii platform to enable cryptocurrency payments by 3rd parties.
Currently, access to these services is by invite only, but in time will be made accessible to the wider developer community.
Integration with the Frii Platform requires secure access tokens provided to authorised and registered entities. 
## Services
### Boarding
In order for Merchants to board with Frii they will be required to demonstrate the following:
-	Proof of business – Location photos and a Utility Bill or statutory document confirming registered business or trading name and address dated in last 6 months
-	Proof of Banking – Bank statement showing business name at registered or trading address
-	Proof of Id – Driving licence or passport and proof of address dated in last 6 months, for each person with 25% or more share in the business or with control over the business’ finances
For security and AML they will also need to provide
-	Estimated Annual non-cash turnover
-	A maximum monthly payments amount
-	Average transaction value
-	A brief description of the business type and activity
#### API (proposed)
Merchants may only be boarded by approved agents and require an access token to submit boarding requests. Target address will also be shared with approved agents and source address will require whitelisting.
Boarding data should be submitted as a Http POST request in JSON format.
The access token should be included in the POST header in the following format:
Authorization: Bearer {access_token}
The following fields are required to board a new merchant to Frii:
 
Field Name	Description
 `site_rep_id`	Agent identifier
 `incorp_date`	Company incorporation date
 `co_reg_number`	Company registration number
 `vat_reg_number`	VAT registration number
 `legal_name`	Legal Name
 `short_name`	Trading Name
 `reg_add1`	Registered Address Line 1
 `reg_add2`	Registered Address Line 2
 `reg_add_town`	Registered Add Town
 `reg_add_county`	Registered Add County / State
 `reg_add_country`	Registered Add Country
 `reg_add_postcode`	Registered Add Postcode / Zip
 `reg_phone`	Registered Phone
 `reg_mobile`	Registered Mobile
 `reg_email`	Registered Email
 `reg_agent`	Registered Agent
 `trad_name`	Tradiing Name
 `trad_add1`	Trading Add Line 1
 `trad_add2`	Trading Add Line 2
 `trad_add_town`	Trading Add Town
 `trad_add_county`	Trading Add County / State
 `trad_add_country`	Trading Add Country
 `trad_add_postcode`	Trading Add Postcode / Zip
 `lat`	Trading Latitude (optional)
 `lon`	Trading Longtitude (optional)
 `trad_tel`	Trading Tel
 `trad_email`	Trading Email
 `trad_description`	Business Description
 `website_url`	Website Url
 `contact_first_name`	Contact First Name
 `contact_last_name`	Contact Last Name
 `contact_tel`	Contact Tel
 `contact_mob`	Contact Mob
 `contact_email`	Contact Email
 `acc_title`	Bank Acc Title
 `acc_first_name`	Bank Acc First Name
 `acc_last_name`	Bank Acc Last Name
 `sort_code`	Bank Acc Sort Code
 `acc_no`	Bank Account No IBAN
 `bank_name`	Bank Name
 `p1_fname`	Principle owner First Name
 `p1_mname`	Principle owner Middle Name
 `p1_surname`	Principle owner Surname
 `p1_dob`	Principle owner DOB - DD/MM/YYYY
 `p1_national_insurance_number`	Principle owner National Insurance No
 `p1_add1`	Principle owner Add Line 1
 `p1_add2`	Principle owner Add Line 2
 `p1_addtown`	Principle owner Add Town
 `p1_addcounty`	Principle owner Add County / state
 `p1_addcountry`	Principle owner Add Country
 `p1_addpcode`	Principle owner Add Postcode / Zip
 `p1_addstatus`	Property status Owned / Rented
 `p1_addyr`	Lived there since - YYYY
 `p1_addmonth`	Lived there since - MM
 `p1_share`	Principle owner Share percentage
 `p2_fname`	Second Principle First Name
 `p2_mname`	Second Principle Middle Name
 `p2_surname`	Second Principle Surname
 `p2_dob`	Second Principler DOB - DD/MM/YYYY
 `p2_national_insurance_number`	Second Principle National Insurance No
 `p2_add1`	Second Principle Add Line 1
 `p2_add2`	Second Principle Add Line 2
 `p2_addtown`	Second Principle Add Town
 `p2_addcounty`	Second Principle Add County / state
 `p2_addcountry`	Second Principle Add Country
 `p2_addpcode`	Second Principle Add Postcode / Zip
 `p2_addstatus`	Property status Owned / Rented
 `p2_addyr`	Lived there since - YYYY
 `p2_addmonth`	Lived there since - MM
 `p2_share`	Second Principle Share percentage
*replace p2 with p3 etc as required	
 `logo_image`	logo in base 64 encoded png 512x512px
 `trading_currency`	Currency code i.e. GBP/USD/EUR
 `media_1`	Business Image 1 in base 64 encoded png 1024x768px
 `media_2`	Business Image 2 in base 64 encoded png 1024x768px
 `media_3`	Business Image 3 in base 64 encoded png 1024x768px
 `media_4`	Business Image 4 in base 64 encoded png 1024x768px

### Payment Transaction
FriiPay transactions can currently only be submitted via FriiPOS applications.
Future iterations will enable transactions to be submitted through approved 3rd party applications.
#### API (proposed)
Payments may only be submitted by approved agents and on behalf of Merchants who have been boarded onto the Frii Platform. Submissions are verified by an access token provided to eligible accounts.
Payment data should be submitted as a Http POST request in JSON format.
The access token should be included in the POST header in the following format:
Authorization: Bearer {access_token}
The following fields are required to submit a payment to Frii:

Field Name	Description
	
‘friipos_id’	FriiPOS identifier
‘value’	Payment amount with 2 decimal places
 
Once submitted, the Frii Platform will return a JSON containing the following fields
 
Field Name	Description
‘status’	‘200’ - ok, or ‘400’ - bad request
‘frii_guid’	FriiPay transaction ID - Varchar(255)
‘qr_png’	Url to display QR code - string
‘websocket_status’	WSS address for status updates - string
‘websocket_validation’	WSS address for transaction validation - string
‘cancel_url’	Url for terminating the transaction

The frii_guid provides a unique reference to the transaction on the frii platform and will be required for any direct queries with Frii.
The QR can be scanned by any Frii/Xumm App to sign and satisfy the associated payload.
The websocket_status will be used to listen for live updates on the transaction status including the following events:
Websocket_status events	Description
expires_in_seconds	Time before payload expires
scanned	QR has been scanned by Frii/Xumm app
signed	Payload has been signed
completed	Transaction completed
rejected	Transaction rejected by user

The websocket_validation will be used for live updates on transaction completion including the following events:
Websocket_validation events	Description
cancelled	Operator / system has cancelled the transaction
validated	The requested funds have been received

The cancel url has been provided to allow the operator to swiftly cancel a transaction that has not been signed yet. NB We strongly advise that any 
