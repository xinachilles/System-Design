- A payment profile can have multiple payment details/instruments
- Each payment detail/instrument belongs to exactly one payment profile
- Customers can manage multiple payment methods under one profile 
- Enable payment method priority and preference 
- Different payment details/instruments within a profile can have different ranks. It used to prioritize and manage the multiple payment methods 
- Tenant refers to the organization entity or business unit that owns the payment data and resources: if you submit a payment at checkout on Amazon, the tenant would be Amazon

### Payment UUID
- If registration is successful, the PSP returns a token to the system (like a payment UUID)
### API 
1. Register 
	- POST /payments/v1/instrument/registerWithoutCharge
	- POST /payments/v1/instrument/registerWithCharge
	
	request
	{
		"paymentRank": 2 
		"clientId"
		"Address"
		"Amount"
		"Country"
		"Language"
		"redirectSuccessURL"
		"redirectFailureURL"	 
	}

2. Create a payment profile, output, profile_id
    POST /payments/v1/profiles  
	
	 GET /payments/v1/profiles/{profile_id}
    
3.  
	- Get all payment instruments/payment methods within a profile
    
	- Get all payment details: GET /payments/v1/profiles/{profileRef}/instruments
		header for the  tenant id:
			
	    not necessary 
    - ~~Get a specific payment instrument: GET /payments/v1/profiles/{profileRef}/instruments/{payment UUID}
    
    4. Add a new payment method
    
	
	~~(delete)~~

    ~~Get or create a payment mode  (or consider it in a separate service)~~
    ~~GET /payments/v1/models/{id}~~




**Payment table**

| payment profile id/profile ref | payment UUID | payment details | payment address | Rank | Currency | Staus | Tenant id  |
| ------------------------------ | ------------ | --------------- | --------------- | ---- | -------- | ----- | ---------- |

payment details column -- object type
{
type: credit_card 
expire date: "1/1/2021"
card type: "VISA"
last 4 digit: "1234"
}

payment address
{
 address1 
}

**Profile Payment table**

| Payment Profile Id |
| ------------------ |


**Payment Session table**

| Registration Session id | Payment profile id | payment UUID | Currency | status | Tenant id |
| ----------------------- | ------------------ | ------------ | -------- | ------ | --------- |

**Payment table and Payment Session table is 1:1** 
