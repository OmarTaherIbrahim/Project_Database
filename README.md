
please read on github alot of effort was put in formatting it for github viewing please!
[here](https://github.com/OmarTaherIbrahim/Project_Database).
# E-Commerce 
This is an E-Commerece website that has users and  sellers This project will aim to use user interaction with the website to benifit the user with recommendation, so the more the user
uses the website the more the recommendations become more accurate.
and we use the multpli user accounts to help the website connect a relation between age, location, purachases, and history.

+ it will __recommend__ based on multiple  relations and attributes like:
1. Customer Buys The Product 
2. Seller Sells The Product 
3. Customer Realation with the seller 
1. Customer recursive realation with Age Group
1. Customer Recursive realation with View History
1. Customer View Product 
1. Customer Bookmark Product
1. Seller Offer on Product 

# The Scope of the project 
In this day and age information is very useful and important even if we don't see the connection
or we aren't using it currently, storing these data is so vital, so with this project we try to
store data as much as we see useful without having storing data that will hogging the storage.

This database aims to be used for an e-commerce site that has multiple types of products and 
many countries to delivery to, since this database can store everything a e-commerce website 
need, from accounts for __users__ ( customers or sellers ), __products__, __orders__, __delvirey service__,
__sections__, __types of products__, and __*views*__ that can be update periodicly to set the __hot__ or 
__new__ products so we can retrive what kind of __products__ we want to __feature__ and __recommend__ to 
our users, and what type of sellers we want to __feature__ because of there good history and reviews.

## About the constraints and the methodology behind it
When we designed these constraints we designed them to be robust and smart, for example most of our 
important tables their __Forgien key__ and their __primary key__ constraints were defined as column level
constriants so it would be hard to drop these constraint vital from any that has drop constraint privileges.
## Advantages of the project to the company
this project is designed in a way to satisfy 1nf, 2nf, 3nf and it stores valuable information
so the project and the database can be expanded on and we could use the information that aren't used as much later on if the company wants to adapt data mining and machine learning with out having to start from the beginning with collecting valuable information

# MYSQL QUERIES
## [Create](Create.md)
##  [Insert](insert.md)
##  [Select queries](selectqueries.md)
## Views
### Derived product
```SQl
### Derived Product
create view derivedproduct as
select product.product_ID as Product_ID, product.Quantity>0 as Availablity,sum(cart.amount) as "Number of sales",avg(ReviewedProducts.rate) as Rating
from product,cart,ReviewedProducts
where product.Product_ID = cart.Product_ID and product.product_id = ReviewedProducts.product_id
group by product_id
order by Product.product_id;
```
this view has info about the product such as:
+ __Availablitiy__ which is if the product is available or not
+ __Number of sale__ is the number of sales of this product for this seller
+ __Rating__ the average rating depending on the reviews that the product gets

### Average Price
```SQl

create view averageprice as
select product.serial_no,avg(product.price) as AveragePrice
from product, Product_type
where Product.serial_no=product_type.serial_no
group by product.Serial_no
order by product.serial_no;
```
+ __avarageprice__ is the price of the product based on it's type  for example: iphone 8 price for seller a is 100 and for seller b is 200 and they are the only sellers 
for iphone 8 then the average price of iphone 8 is 150

### Hot Products
```SQl

create view Hot as
select product.Product_ID as HotProduct 
from Product,derivedproduct 
where DATEDIFF(current_date,product.PublishDate)<120
and derivedproduct.Product_ID=product.Product_ID
and derivedproduct.rating >3.5;
```
+ this view will have the products that are hot(age<120days(4 months) and has a rating above 3.5)
### New products
```SQl
create view New as
select product.Product_ID as NewProduct 
from Product,derivedproduct 
where DATEDIFF(current_date,product.PublishDate)<30
and derivedproduct.Product_ID=product.Product_ID
and derivedproduct.rating >3.5;
```
+ this view will have the products that are hot(age<30days(1 month) and has a rating above 3.5)
### Best products of the year
```SQl
create view BestOfYear as
select product.Product_ID as BestOfYearProduct 
from Product,derivedproduct 
where DATEDIFF(current_date,product.PublishDate)<365
and derivedproduct.Product_ID=product.Product_ID
and derivedproduct.rating >4;
select Product_ID as  from Product where DATEDIFF(current_date,product.PublishDate)<365;
```
+ this view will have the products that are hot(age<365days(1 year) and has a rating above 4)



##drop statements

```sql
drop table available_country,
Bookmark,
cart,
Colors,
customer,
Customer_order,
Delivers_To,
Delivery_service,
offer,
Product,
realation,
ReviewedProducts,
Section,
Seller,
share_view_history,
Sizes,
Tags,
Type,
User_Account,
ViewedProduct;
```