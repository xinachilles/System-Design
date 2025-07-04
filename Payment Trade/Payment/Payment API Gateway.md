1. Authentication using SSAK (self-service API keys)
	1. Extract credentials from the header 
	2. Validates the API key against the SSAK system 
	3. May need to update the header for the downstream system 
2.  Authorization using SSAL (self-service Authorization layer)
	1. Take the authenticated client name 
	2. Search the DB to find out the tenan_ref , which indicates which tenant's data the client can access
	3. Add this tenan_ref to the header, and for the downstream system 
	4. Remove some information from the header request, like agent-name .. 
	5. (Optional) Exposing only the external endpoints through the gateway and aggregating them in a single Swagger page

### Database 

| client name | tenan_ref |
| ----------- | --------- |
- After the client is authenticated, the payment API gateway looks up the tenan_ref for that client
- Client name and tenan_ref is 1:1 
- Client name is the partition key 
