-  The upstream service, [[Payment API Gateway]], calls the API in payment gateway management  
-  After calling the API, publish an event to SQS, since SQS has a dead letter queue. Payment Gateway Management is also listed in the SQS queue, allowing the invocation of different PSP APIs based on the event type.

### GatewayRequestUUID
- Generated on the client side (front-end or calling service) for each payment attempt 
- Used to identify the specific charge request, used for idempotency 

## API 

- POST /payments/v1/payment/charge  
	request {
	 [[Payment Registration Service#Payment UUID]],
	 amount
		 {
			 amountLocal
			 currency 
		 }
	 [[Payment Gateway Management#GatewayRequestUUID]]	
	
	}


- POST /payments/v1/payment/refund 

Charge table

| Transaction id | Payment token | AmountLocal | Currency | ProfileID | TenanRef | Status |
| -------------- | ------------- | ----------- | -------- | --------- | -------- | ------ |
status: Initiated, acked (acknowledged/success), denied, nacked (not acknowledged/failure) , approved, aborted
