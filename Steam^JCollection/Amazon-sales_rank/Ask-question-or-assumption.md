# Ask question or assumption

Created: 2021-02-04 17:29:04 -0600

Modified: 2021-02-04 18:48:58 -0600

---

1.  Ask question or assumption

How many user view the past weeks most popular product by category



Assumption:



Items cannot change categories



Traffic is not evenly distributed

Results must be updated hourly



- 10 million products
- 1000 categories
- 1 billion transactions per month
- 100 billion read requests per month







We assume after user click the order button or the payment is completed , we use API gateway for log generation, we will generate a key value log like



User id# time stamp --- category#product id



We will use these logs for counting how many times each product was purched.





how map reduce sort the result



![class SalesRanker(MRJob): def within_past_week(self, timestamp): """Return True if timestamp is within past week, def mapper(self, line) : False otherwise. """Parse each log line, extract and transform relevant lines. Emit key value pairs of the form: (categoryl, (catego ry2, (catego ry2, (catego ryl, (catego ry2, (catego ryl, productl) , productl) , productl) , product2) , product3) , product4) , timestamp, product_id, 2 2 3 7 category_id, quantity, total_price, seller_id, buyer_id = line.split( 't') if self.within_past_week(timestamp): yield (category_id, product_id), quantity def reducer(self, key, value): """Sum values for each key. (categoryl, (catego ry2, (categoryl, (catego ry2, (catego ryl, productl) , productl) , product2) , product3) , product4) , yield key, sum(values) def mapper_sort(self, key, 2 3 3 7 value): """Construct key to ensure proper sorting. Transform key and value to the form: (catego ryl, (catego ry2, (catego ryl, (catego ry2, (catego ryl, 2), 3), 3), 7), 1), productl productl p roduct2 p roduct3 p roduct4 ](../../media/Steam^JCollection-Amazon-sales_rank-Ask-question-or-assumption-image1.png){width="5.0in" height="3.9479166666666665in"}



![The shuffle/sort step of MapReduce will then do a distributed sort on the keys, resulting in: (categoryl, 1), product4 (categoryl, 2), productl (categoryl, 3), product2 (category2, 3), productl (category2, 7), product3 category_id, product_id = key quantity = value yield (category_id, quantity), product_id def reducer_identity(self, key, value) : yield key, value def steps (self): """Run the map and reduce steps.""" return self. mr(mappe . mapper, reducer---self. reducer) , self. mr(mappe . mapper_sort , reducer---self. reducer_identity) , The result would be the following sorted list, which we could insert into the sa les_rank table: ( categoryl, (categoryl, (categoryl, (category2, ( category2, 1), 2), 3), 3), 7), product4 productl product2 productl product3 ](../../media/Steam^JCollection-Amazon-sales_rank-Ask-question-or-assumption-image2.png){width="5.0in" height="3.3333333333333335in"}






