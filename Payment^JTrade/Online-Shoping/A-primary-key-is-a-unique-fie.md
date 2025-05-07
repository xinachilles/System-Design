# A primary key is a unique field on a table but it is special in the sense that the table considers that row as its key. This means that other tables can use this field to create foreign key relationships to themselves.

Created: 2018-01-31 00:38:37 -0600

Modified: 2018-01-31 00:38:50 -0600

---

A primary key is a unique field on a table but it is special in the sense that the table considers that row as its key. This means that other tables can use this field to create foreign key relationships to themselves.

primary key also create a clustted index for this table





A unique constraint simply means that a particular field must be unique.



Department table

Department id, Department name, description



Category table

Category id, Department id(FK), name, description



Production table

Production id, name, description,price







ProductionCategory table

production id

Category id





Attribution table (the name of the attributes, such as Size or Color)

Attribution id

Name



AttributeValue table (contains the possible attribute values for each attribute group)

AttributeValue id

Attribution id

int value



ProductAttributeValue is an associate table implementing a Many-to-Many relationship

between the Product and AttributeValue tables, with (ProductID, AttributeValueID)



Shopping Car

CarID

ProductID

Attributes

Quality

DataAdd





Order table

OrderID, CustomeID,DataCreated,DateShip,Status,Comments,,AuthCode (The authentication code used to complete the

customer credit card transaction) ,Referene (The unique reference code of the customer credit

card transaction)



status:

vertifed( after payment processed),Completed ( already shiped),Canceld



Order Detail

OrderID, ProductID,Quality,



Tax table

Tax id, Tax Type, Precatge



Shopping table

Shipping id, Shipping type, Shipping cost,ShippingRegionID



Audit table

Audit id, orderid, datestamp, message,





address table

customer id, address id, start data, end data, state, city, streee,zipcode,last update time





payment table



payment id, customer id, payment type(credit type, debit type..), amount, payment date










