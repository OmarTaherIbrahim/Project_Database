note you need to [create tables](Create.md) and [insert values](insert.md) 
# Single Queries
### number of products sold of a certain product
```sql
select sum(amount) from cart 
where product_id=1;# number 1 can 
```
#### Output
```
SaraAhmmad
Alyx1478945
Jaxon7894
Sharif5496
asd5as6d65
dasdas1651
ZackRedmond
AmeeraHumphries
ZaneHenson
KendrickFernandez5496
HiraDavies
KymaniPetty
```
### username of users in a certain country
```sql
select Username from user_account
where country='Usa';
```
#### Output
```
SaraAhmmad
Alyx1478945
Jaxon7894
Sharif5496
asd5as6d65
dasdas1651
ZackRedmond
AmeeraHumphries
ZaneHenson
KendrickFernandez5496
HiraDavies
KymaniPetty
```
### username of users who have a certain birthday
```sql
select username ,birthdate from user_account
where birthdate='1988-08-02';
```
#### Output
```
SaraAhmmad      1988-08-02
Alyx1478945     1988-08-02
Sharif5496      1988-08-02
```
### username of users who are customer
```sql
select username from user_account
where AccountType='C';
```
#### Output
```
Fadwa
Dahook159
ZackRedmond
AmeeraHumphries
ZaneHenson
DukeWade
DanniellaCarver1654
KendrickFernandez5496
HiraDavies
KymaniPetty
```
### username of users who are sellers
```sql
select username from user_account
where AccountType='S';
```
#### Output
```
Ahmadqasho
MohammadAhmmad
SaraAhmmad
Alyx1478945
Jaxon7894
NathanialWicks
Omar1654
Sharif5496
asd5as6d65
dasdas1651
```
### product_id and their price that is sold by a certain seller
```sql
select Product_ID,price,Quantity from Product
where Owner_ID=2;
```
#### Output
```
Product_ID  price   Quantity
2           700     40
12          200     12
```
### product_id and the price of a product that is between a certain range
```sql
select Product_ID,price from Product
where price between 0 and 700;
```
#### Output
```
1   600
2   700
3   500
7   700
9   650
12  200
13  400
15  100
```
### product_id of products that are avaliable
```sql
select Product_ID,price from Product
where Quantity >0;
```
#### Output
```
Product_ID  price
1           600
2           700
3           500
4           1500
5           2000
6           1000
7           700
8           1200
9           650
10          1200
11          1000
12          200
13          400
14          1200
15          100
```
### prouduct_id of products that are not avaliable
```sql
select Product_ID,price from Product
where Quantity <=0;
```
#### Output
```
Product_ID  price
16          1000

```

### bank account for all users
```sql
select User_Account.username,User_Account.BankAccount
from User_Account;
```
#### Output
```
username                        Password
Ahmadqasho                      1234567899876543
MohammadAhmmad                  1234566899876543
SaraAhmmad                      1254566899226543
Alyx1478945                     7412545823310946
Jaxon7894                       7628078020066261
NathanialWicks                  6164049448896358
Omar1654                        1831637873382355
Sharif5496                      7992577620901883
asd5as6d65                      0372356675801587
dasdas1651                      5135600799104997
Fadwa                           1234567899876543
Dahook159                       0279605915676901
ZackRedmond                     4481007764554591
AmeeraHumphries                 5288594769914994
ZaneHenson                      2198363754951335
DukeWade                        7070396618482580
DanniellaCarver1654             8341999062632854
KendrickFernandez5496           6714318432063381
HiraDavies                      4998110279823454
KymaniPetty                     3552291457493930
```
### Email password account type 
```sql
select User_Account.username,User_Account.account_pass as Password,User_Account.AccountType
from User_Account;
```
#### Output
```

username                Password                AccountType
Ahmadqasho              Ahmad1998                   S
MohammadAhmmad           Mohammad1998               S
SaraAhmmad              asd48as133                  S
Alyx1478945             asd05420                    S
Jaxon7894               Jaxonia123                  S
NathanialWicks          Nathanial2121               S
Omar1654                Omar6545                    S
Sharif5496              Shar1522i                   S
asd5as6d65              asdasd                      S
dasdas1651              sf2v0sf                     S
Fadwa                   Fadwa123                    C
Dahook159               Dahooj15465                 C
ZackRedmond             ZackRedmond84630            C
AmeeraHumphries         asd05420                    C
ZaneHenson              ZaneHenson1456              C
DukeWade                Duke Wadel2121              C
DanniellaCarver1654     Danniell6545                C
KendrickFernandez5496   Kendrick1654                C
HiraDavies              Hira1654                    C
KymaniPetty             sf2v0sf                     C
```

# Multiple Queries
### number of products sold by a seller 
```sql
select sum(amount) as "number of products sold by a seller" from cart,product 
where cart.Product_ID=product.Product_ID
and product.Owner_ID=1;# 1 can be a variable inserted by client's program
```
#### Output
```

number of products sold by a seller
4
```
### number of products bought by a customer
```sql
select sum(amount) as "number of products bought by a customer"
from cart,product,customer_order
where cart.Product_ID=product.Product_ID
and cart.reciet_no=Customer_order.reciet_no
and Customer_order.Customer_ID=11;# 1 can be a variable inserted by client's program
```
#### Output
```
number of products bought by a customer
15
```
### number of product a certain customer bought of a certain type by id
```sql
select product_type.TypeName as Type,
sum(amount) as "number of products(certain type) bought by a customer"
from cart,product,customer_order,product_type
where cart.Product_ID=product.Product_ID
and cart.reciet_no=Customer_order.reciet_no
and product_type.serial_no=Product.Serial_no
and Customer_order.Customer_ID=11
and product.serial_no=2;# 1 can be a variable inserted by client's program
```
#### Output
```
Type    number of products(certain type) bought by a customer
Mac         4
```
### number of product a certain customer bought of a certain type by name
```sql
select product_type.serial_no as Type,
sum(amount) as "number of products(certain type) bought by a customer by name"
from cart,product,customer_order,product_type
where cart.Product_ID=product.Product_ID
and cart.reciet_no=Customer_order.reciet_no
and product_type.serial_no=Product.Serial_no
and Customer_order.Customer_ID=11
and product_type.TypeName='Mac';# 1 can be a variable inserted by client's program
```
#### Output
```
Type    number of products(certain type) bought by a customer by name
2       4
```
### number of product a certain customer bought from a certain section
```sql
select Section.SectionName as Type,
sum(amount) as "number of products(certain section) bought by a customer by name"
from cart,product,customer_order,product_type,Section
where cart.Product_ID=product.Product_ID
and cart.reciet_no=Customer_order.reciet_no
and product_type.serial_no=Product.Serial_no
and Section.SectionName=product_type.SectionName
and Customer_order.Customer_ID=11
and Section.SectionName='computers';
```
#### Output
```
Type        number of products(certain section) bought by a customer by name
computers   15
```
### number of products sold this year
```sql
select sum(amount) as "number of products sold this year"
from cart, product, Customer_order
where cart.reciet_no=Customer_order.reciet_no
and Product.Product_ID = cart.Product_ID
and DATEDIFF(current_date,customer_order.date_of_order)<=365;
```
#### Output
```
number of products sold this year
22
```
### number of products sold this month
```sql
select sum(amount) as "number of products sold this month"
from cart, product, Customer_order
where cart.reciet_no=Customer_order.reciet_no
and Product.Product_ID = cart.Product_ID
and DATEDIFF(current_date,customer_order.date_of_order)<=30;
```
#### Output
```
number of products sold this month
1
```
### number of products sold this month from a section
```sql
select Section.SectionName , sum(amount)as "number of products sold this month"
from cart, product, Customer_order,Section,product_type
where cart.reciet_no=Customer_order.reciet_no
and Product.Product_ID = cart.Product_ID
and Product.serial_no=product_type.serial_no
and Section.SectionName=product_type.sectionname
and DATEDIFF(current_date,customer_order.date_of_order)<=30
and Section.SectionName='mobiles'
group by Section.sectionname;
```
#### Output
```
SectionName     number of products sold this month
Mobiles         1
```
### number of products sold this month from a certain type
```sql
select product_type.TypeName , sum(amount)as "number of products sold this month"
from cart, product, Customer_order,product_type
where cart.reciet_no=Customer_order.reciet_no
and Product.Product_ID = cart.Product_ID
and Product.serial_no=product_type.serial_no
and DATEDIFF(current_date,customer_order.date_of_order)<=30
and product_type.TypeName='Samsung'# this can be changed to suit the type needed
group by product_type.TypeName;
```
#### Output
```

TypeName    number of products sold this month
Samsung     1
```
## customer info 
### phone numbers
```sql
select User_Account.username,User_Account.phonenumber
from User_Account,Customer
where customer.Customer_ID=user_account.userid;
```
#### Output
```
username                        phonenumber
Fadwa                           096740566157
Dahook159                       0962730462157
ZackRedmond                     0012799965477
AmeeraHumphries                 0012798462157
ZaneHenson                      0012025550154
DukeWade                        0962798466127
DanniellaCarver1654             0962784462157
KendrickFernandez5496           0012035552174
HiraDavies                      0012023164687
KymaniPetty                     001286550112
```
### phone numbers of a specific user that is only a customer
```sql
select User_Account.username,User_Account.phonenumber
from User_Account,Customer
where customer.Customer_ID=user_account.userid
and user_account.username='Fadwa';
```
#### Output
```
username    phonenumber
Fadwa       096740566157
```
### phone numbers of a specific user that is only a seller
```sql
select User_Account.username,User_Account.phonenumber
from User_Account,seller
where seller.seller_id=user_account.userid
and User_Account.username='SaraAhmmad';
```
#### Output
```
username        phonenumber
SaraAhmmad      0012798462157
```
### bank account for all customers
```sql
select User_Account.username,User_Account.BankAccount
from User_Account,customer
where User_Account.userid=customer.Customer_ID;
```
#### Output
```
username                BankAccount
Fadwa                   1234567899876543
Dahook159               0279605915676901
ZackRedmond             4481007764554591
AmeeraHumphries         5288594769914994
ZaneHenson              2198363754951335
DukeWade                7070396618482580
DanniellaCarver1654     8341999062632854
KendrickFernandez5496   6714318432063381
HiraDavies              4998110279823454
KymaniPetty             3552291457493930
```
## Product info
### tags of a product
```sql
select product_type.TypeName "Product Name",Tags.Tags "Tag"
from Product,product_type,Tags
where product_type.Serial_no=product.serial_no
and Product.Product_ID=Tags.Product_ID
and product_type.TypeName='Mac';
```
#### Output
```
Product Name    Tag
Mac             2019
Mac             Best Laptops
Mac             Mac
```
### sizes of a product
```sql
select product_type.TypeName "Product Name",Sizes.Size "Sizes"
from Product,product_type,Sizes
where product_type.Serial_no=product.serial_no
and Product.Product_ID=Sizes.Product_ID
and product_type.TypeName='American-Eagle';
```
#### Output
```

Product Name        Sizes
American-Eagle      L
American-Eagle      M
American-Eagle      S
American-Eagle      XL
American-Eagle      XLL
American-Eagle      L
American-Eagle      M
American-Eagle      S
American-Eagle      XL
```
### colors of a product
```sql
select product_type.TypeName "Product Name",Colors.Color "Color"
from Product,product_type,Colors
where product_type.Serial_no=product.serial_no
and Product.Product_ID=Colors.Product_ID
and product_Type.TypeName='Mac';
```
#### Output
```
Product Name    Color
Mac             Golden
Mac             Silver
Mac             Black
```
### delivers_to of a product
```sql
select product_type.TypeName "Product Name",delivers_to.delivers_to "Deliver to"
from Product,product_type,delivers_to
where product_type.Serial_no=product.serial_no
and Product.Product_ID=delivers_to.Product_ID
and product_Type.TypeName='Mac';
```
#### Output
```
oduct Name      Deliver to
Mac             Usa
Mac             Jordan
Mac             Jordan
```
### the products that are delivered to certain locations
```sql
select product_type.TypeName "Product Name",delivers_to.delivers_to "Deliver locations"
from Product,product_type,delivers_to
where product_type.Serial_no=product.serial_no
and Product.Product_ID=delivers_to.Product_ID
and Delivers_To.Delivers_To='Jordan';
```
#### Output
```

Product Name    Deliver locations
Lenovo          Jordan
Mac             Jordan
Mac             Jordan
```
