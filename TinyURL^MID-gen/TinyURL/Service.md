# Service 

Created: 2017-10-09 16:33:42 -0600

Modified: 2017-10-09 16:40:26 -0600

---

![](../../media/TinyURL^MID-gen-TinyURL-Service-image1.png){width="10.083333333333334in" height="5.739583333333333in"}



creatURL(original_url, custom_alias=None user_name=None, expire_date=None)





**Parameters:**



original_url (string): Original URL to be shortened.

custom_alias (string): Optional custom key for the URL.

user_name (string): Optional user name to be used in encoding.

expire_date (string): Optional expiration date for the shortened URL.



**Returns:**(string)

A successful insertion returns the shortened URL, otherwise, returns an error code.



decode(short_url)





