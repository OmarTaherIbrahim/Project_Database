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

# MYSQL QUERIES
## Creating Table 
``` sql
create table User_Account(
    UserId int auto_increment,
    Email varchar(30) not null,
    username varchar(30) not null ,
    account_pass varchar(16) not null,
    Backup_Email varchar(30) not null,
    phonenumber varchar(15) not null,
    address varchar(20) not null,
    country varchar(15),
    BankAccount char(16), # the way of payment like visa
    birthdate date,       # 8 because like 01-01-1998
    AccountType varchar(1) not null,
    primary key (userId),
    constraint UserAccount_UN_Unique unique(username),
    constraint UserAccount_Email_Unique unique(Email)
);

create table Seller(
    seller_id int not null,
    rating int check(rating>0 and  rating <6),
    primary key(seller_id),
    foreign key(seller_id) 
    references User_Account(userid)
);	

create table customer(
	Customer_id int not null ,
	constraint  primary key (customer_id),
	constraint  foreign  key (customer_id) references User_Account(userId)
);

create table share_view_history(
    customer_id int not null,
    second_customer_id int not null ,
    Accuracy decimal(3,2) ,
    constraint shareview_Accuracy_check check(Accuracy >=0 and Accurace <=1), 
    constraint sharview_PK primary key(customer_id,second_customer_id),
    constraint shareview_custid_FK foreign key (customer_id) references customer(customer_id),
    constraint shareview_2custid_FK foreign key (second_customer_id) references customer(customer_id)
);

create table realation(
    customer_id int not null ,
    seller_id int not null ,
    constraint realation_PK primary key (customer_id,seller_id),
    constraint realation_seller_FK foreign  key (seller_id) references Seller(Seller_id),
    constraint realation_customer_Fk foreign  key (customer_id) references customer(customer_id)
);

create table available_country(
    seller_id int not null,
    available_countrys varchar(20) not null ,
    constraint Availablecountry_Pk primary key(seller_id),
    constraint Availablecountry_seller_Fk foreign key(seller_id) references seller(seller_id)
);
    
		
create table Customer_order (
    reciet_no int not null auto_increment,
    totalprice float default 0,
    totalpricewithtax float default 0,
    Date_of_order date ,
    tax float,
    customer_id int not null ,
    constraint  customerorder_check check (tax >=0),
    constraint  primary key(reciet_no) ,
    constraint  foreign key (customer_id) references customer(customer_id)
);
		
create table Delivery_service(
    reciet_no int not null ,
    Deleivery_time date ,
    price float ,
    constraint Delivery_Check check(price > 0),
    constraint Deleivery_PK primary key (reciet_no),
    constraint Delivery_Fk foreign key (reciet_no) references customer_order(reciet_no)
);
		
create table Section(
    SectionName varchar(15) not null,
    ParentSection varchar(15) default null,
    constraint Section_pk PRIMARY key(SectionName),
    constraint Section_Parent_fk foreign key (ParentSection) references Section(SectionName) on delete set null,
    unique(SectionName)
);

create table Product_Type(
    Serial_no int auto_increment primary key,
    TypeName varchar(15) not null unique,
    SectionName varchar(15)not null,
    foreign key(SectionName) references Section(SectionName) 
);

create table Product(
    Product_ID int auto_increment primary key,
    serial_no int ,
    foreign key(serial_no) references Product_Type(Serial_no),
    Price int not null,
    PublishDate Date not null,
    Country varchar(20) not null,
    Owner_ID int not null,
    Quantity int not null check (Quantity>=0)
);

create table cart(
    reciet_no int not null ,
    product_id int not null ,
    amount int ,
    constraint Cart_PK primary key (reciet_no,product_id),
    constraint Cart_order_FK foreign key (reciet_no) references customer_order(reciet_no),
    constraint Cart_product_FK foreign key(product_id) references product(product_id)
);
    
create table offer ( 
    product_id int not null ,
    seller_id int not null ,
    datestart date ,
    dateend date,
    percentage decimal (3,2) default 0 ,
    constraint Offer_Pk primary key (product_id,seller_id),
    constraint Offer_product_FK foreign key(product_id) references product(product_id),
    constraint Offer_sellerid_FK foreign key(seller_id) references seller(seller_id)
);

create table Tags (
    Tags varchar(15) ,
    Product_ID int,
    foreign key(Product_ID) references Product(Product_ID),
    primary key (Tags,Product_ID)
);

create table Sizes (
    Size varchar(15) ,
    Product_ID int ,
    foreign key(Product_ID) references Product(Product_ID),
    primary key (Size,Product_ID)
);

create table Delivers_To (
    Delivers_To varchar(15),
    Product_ID int,
    foreign key(Product_ID) references Product(Product_ID),
    primary key (Delivers_To,Product_ID)
);

create table Colors(
    Color varchar(15) ,
    Product_ID int,
    foreign key(Product_ID) references Product(Product_ID),
    primary key (Color,Product_ID)
);

create table ViewedProduct(
    Customer_ID int,
    Product_ID int,
    primary key(Customer_ID,Product_ID)
);

create table Bookmark(
    Customer_ID int,
    Product_ID int,
    primary key(Customer_ID,Product_ID)
);

create table ReviewedProducts(
    Customer_ID int,
    Product_ID int,
    ReviewDescription varchar(150),
    Rate int,
    primary key(Customer_ID,Product_ID),
    constraint check_rate_reviewed check(Rate>0 and Rate<6)
);

```
## Views
```SQl
### Hot products
create view Hot as
select Product_ID as HotProduct from Product where DATEDIFF(current_date,product.PublishDate)<120;
### New products
create view New as
select Product_ID as NewProduct from Product where DATEDIFF(current_date,product.PublishDate)<30;
### Best products of the year
create view BestOfYear as
select Product_ID as BestOfYearProduct from Product where DATEDIFF(current_date,product.PublishDate)<365;
```

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
#insert statements 
```sql
### insert here 
### Users
insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Ahmad123@gmail.com','Ahmadqasho','Ahmad1998','ahmadgasho@gmail.com','0962798463157','Universtystreet','Jordan','1234567899876543','1998-7-16','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Mohammad456@gmail.com','MohammadAhmmad','Mohammad1998','MohammadAhmad@gmail.com','0962798462157','Universtystreet','Jordan','1234566899876543','1978-8-16','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('SaraAhmad@gmail.com','SaraAhmmad','asd48as133','SaraAhmad@gmail.com','0012798462157','51Green BayWI 54302','Usa','1254566899226543','1988-8-2','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('AlyxProctor134@gmail.com','Alyx1478945','asd05420','AlyxProctor164@gmail.com','0012798462157','51Green BayWI 54302','Usa','7412545823310946','1988-8-2','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('JaxonMichel@gmail.com','Jaxon7894','Jaxonia123','Jaxonia1784@gmail.com','0012398462257','William MI 49464','Usa','7628078020066261','1984-2-12','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('NathanialWicks@yahoo.com','NathanialWicks','Nathanial2121','Nathanial467@gmail.com','0962798465147','Abdoon','Jordan','6164049448896358','1970-1-30','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Omar@hotmail.com','Omar1654','Omar6545','MohammadAhmad@gmail.com','0962784462157','TlaAlAlit','Jordan','1831637873382355','1978-8-16','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Sharif0654@gmail.com','Sharif5496','Shar1522i','Sharif150@gmail.com','0012798462157','51Green BayWI 54302','Usa','7992577620901883','1988-8-2','S');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Majd1564@gmail.com','asd5as6d65','asdasd','SaraAhmad@gmail.com','0012798462157','Nashville, TN 37205','Usa','0372356675801587','1992-11-2','S');

 insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Alexandarahaha@gmail.com','dasdas1651','sf2v0sf','Alexandra467@gmail.com','0012797462156','William MI 49464','Usa','5135600799104997','1984-2-12','S');
 insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Fadwa45@gmail.com','Fadwa','Fadwa123','Fadwa4545@gmail.com','096740566157','shmesani','Jordan','1234567899876543','1991-1-15','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('Dahook456@gmail.com','Dahook159','Dahooj15465','Dahook456@hotmail.com','0962730462157','Universtystreet','Jordan','0279605915676901','1968-8-12','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('ZackRedmond@gmail.com','ZackRedmond','ZackRedmond84630','ZackRedmond4567@hotmail.com','0012799965477','51Green BayWI 54302','Usa','4481007764554591','1984-8-2','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('AmeeraHumphries@gmail.com','AmeeraHumphries','asd05420','Ameera Humphries164@gmail.com','0012798462157','Canal OH43110','Usa','5288594769914994','1986-8-23','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('ZaneHenson1122@gmail.com','ZaneHenson','ZaneHenson1456','ZaneHenson147@gmail.com','0012025550154','William MI 49464','Usa','2198363754951335','1981-2-28','C');

 insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('DukeWade@yahoo.com','DukeWade','Duke Wadel2121','Duke Wade1467@gmail.com','0962798466127','Irbid','Jordan','7070396618482580','1972-1-30','C');

 insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('DanniellaCarver@hotmail.com','DanniellaCarver1654','Danniell6545','DanniellaCarver1654@gmail.com','0962784462157','Gardenz','Jordan','8341999062632854','1998-10-8','C');

 insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('KendrickFernandez@gmail.com','KendrickFernandez5496','Kendrick1654','KendrickFernandez16@gmail.com','0012035552174','Maspeth, NY 11378','Usa','6714318432063381','1985-3-12','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('HiraDavies145@gmail.com','HiraDavies','Hira1654','HiraDavies1234@gmail.com','0012023164687','New Rochelle NY10801','Usa','4998110279823454','1994-4-5','C');

insert into User_Account(Email,Username,account_pass,Backup_Email,phoneNumber,address,country,bankaccount,birthdate,accounttype)
values('KymaniPetty@gmail.com','KymaniPetty','sf2v0sf','KymaniPetty165@gmail.com','001286550112','Owatonna, MN 55060','Usa','3552291457493930','1983-6-8','C');



###Sellers

insert into seller(seller_id,rating)
values('1','5');
insert into seller(seller_id,rating)
values('2','5');
insert into seller(seller_id,rating)
values('3','5');
insert into seller(seller_id,rating)
values('4','5');
insert into seller(seller_id,rating)
values('5','5');
insert into seller(seller_id,rating)
values('6','5');
insert into seller(seller_id,rating)
values('7','5');
insert into seller(seller_id,rating)
values('8','5');
insert into seller(seller_id,rating)
values('9','5');
insert into seller(seller_id,rating)
values('10','5');





### Customers 
insert into customer(customer_id)
values('11');
insert into customer(customer_id)
values('12');
insert into customer(customer_id)
values('13');
insert into customer(customer_id)
values('14');
insert into customer(customer_id)
values('15');
insert into customer(customer_id)
values('16');
insert into customer(customer_id)
values('17');
insert into customer(customer_id)
values('18');
insert into customer(customer_id)
values('19');
insert into customer(customer_id)
values('20');





### Sections
insert into section(sectionname,parentsection)
values('Electronics','Electronics');
insert into section(sectionname,parentsection)
values('Accessories','Accessories');
insert into section(sectionname,parentsection)
values('Clothes','Clothes');
insert into section(sectionname,parentsection)
values('computers','Electronics');
insert into section(sectionname,parentsection)
values('Mobiles','Electronics');
insert into section
values('Wallets','Accessories');
insert into section(sectionname,parentsection)
values('T-Shirts','Clothes');





### Types

insert into Product_Type(serial_no,typename,sectionname) 
values('1','Lenovo','computers');
insert into Product_Type(serial_no,typename,sectionname)
values('2','Mac','computers');
insert into Product_Type(serial_no,typename,sectionname)
values('3','Samsung','Mobiles');
insert into Product_Type(serial_no,typename,sectionname)
values('4','iPhone8','Mobiles');
insert into Product_Type(serial_no,typename,sectionname)
values('5','rolex watches','Wallets');
insert into Product_Type(serial_no,typename,sectionname)
values('6','American-Eagle','T-Shirts');



### Customer_orders
insert into customer_order(reciet_no,totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('11','700','717.5','2018-8-11','2.5','11'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('500','525','2018-6-20','5','11'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1500','1650','2018-3-30','10','11'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1000','1025','2018-7-11','2.5','12'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1200','1260','2018-4-20','5','13'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('600','660','2018-12-30','10','14'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1000','1025','2018-8-16','2.5','14'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1200','1260','2018-6-20','5','15'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('600','660','2018-3-1','10','16'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('2000','2700','2018-8-11','35','12'); 
insert into customer_order(totalprice,totalpricewithtax,date_of_order,tax,customer_id)
values('1300','1495','2018-6-20','15','13'); 



###Products

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
 values('1','600','2018-1-1','Jordan','1','20');

 insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('1','700','2018-7-4','Usa','2','40');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('1','500','2018-8-16','Japan','3','15');

 insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('2','1500','2018-7-12','Jordan','4','16');

 insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('2','2000','2016-6-22','Usa','5','30');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('2','1000','2017-12-2','Japan','6','10');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('3','700','2018-10-16','Japan','7','50');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('3','1200','2018-11-2','Usa','8','20');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('3','650','2018-12-21','Japan','9','12');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('4','1200','2018-7-21','Jordan','7','50');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('4','1000','2018-8-2','Usa','7','20');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('6','200','2018-3-13','Japan','2','12');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('5','400','2018-10-16','Japan','1','50');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('5','1200','2018-11-2','Usa','3','20');

insert into product(serial_no,price,publishdate,country,owner_id,Quantity)
values('6','100','2018-8-15','Japan','4','30');





###Delivery Service
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('11','2018-9-12','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('12','2018-7-20','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('13','2018-4-20','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('14','2018-8-11','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('15','2018-5-20','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('16','2019-1-30','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('17','2018-7-16','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('18','2018-7-20','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('19','2018-4-1','200');
insert into Delivery_service (reciet_no,Deleivery_time,price)
values('20','2018-9-11','200');





###Cart
insert into cart(reciet_no,product_id,amount)
values('11','1','2');
insert into cart(reciet_no,product_id,amount)
values('11','2','4');
insert into cart(reciet_no,product_id,amount)
values('11','3','3');
insert into cart(reciet_no,product_id,amount)
values('12','1','2');
insert into cart(reciet_no,product_id,amount)
values('13','5','4');
insert into cart(reciet_no,product_id,amount)
values('14','4','3');
insert into cart(reciet_no,product_id,amount)
values('15','9','2');
insert into cart(reciet_no,product_id,amount)
values('16','8','1');
insert into cart(reciet_no,product_id,amount)
values('17','7','1');





###Offer
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('1','1','2018-2-1','2018-3-1','0.15');
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('3','4','2018-8-1','2018-9-1','0.30');
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('2','1','2018-8-4','2018-9-4','0.15');
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('6','4','2017-4-2','2017-4-30','0.10');
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('8','1','2018-11-2','2018-11-10','0.20');
insert into Offer(product_id,seller_id,datestart,dateend,percentage)
values('7','4','2018-11-16','2018-11-20','0.50');





###Tags
insert into tags(Tags,Product_id)
values('Lenovo','1');
insert into tags(Tags,Product_id)
values('Computers','1');
insert into tags(Tags,Product_id)
values('Brand New','1');
insert into tags(Tags,Product_id)
values('2018','1');
insert into tags(Tags,Product_id)
values('Mac','4');
insert into tags(Tags,Product_id)
values('Best Laptops','4');
insert into tags(Tags,Product_id)
values('2019','4');
insert into tags(Tags,Product_id)
values('Samsung','7');
insert into tags(Tags,Product_id)
values('Korean','7');
insert into tags(Tags,Product_id)
values('iphone8','10');
insert into tags(Tags,Product_id)
values('iphone','10');
insert into tags(Tags,Product_id)
values('Mobiles','10');
insert into tags(Tags,Product_id)
values('2018','10');





###Sizes
insert into sizes(size,product_id)
values('S','15');
insert into sizes(size,product_id)
values('M','15');
insert into sizes(size,product_id)
values('L','15');
insert into sizes(size,product_id)
values('XL','15');
insert into sizes(size,product_id)
values('S','12');
insert into sizes(size,product_id)
values('M','12');
insert into sizes(size,product_id)
values('L','12');
insert into sizes(size,product_id)
values('XL','12');
insert into sizes(size,product_id)
values('XLL','12');






###Delivers_To
insert into Delivers_To(Delivers_To,product_id)
values('Jordan','1');
insert into Delivers_To(Delivers_To,product_id)
values('Japan','2');
insert into Delivers_To(Delivers_To,product_id)
values('Usa','3');
insert into Delivers_To(Delivers_To,product_id)
values('Usa','4');
insert into Delivers_To(Delivers_To,product_id)
values('Jordan','5');
insert into Delivers_To(Delivers_To,product_id)
values('Jordan','6');
insert into Delivers_To(Delivers_To,product_id)
values('Usa','7');
insert into Delivers_To(Delivers_To,product_id)
values('Usa','8');
insert into Delivers_To(Delivers_To,product_id)
values('Japan','9');
insert into Delivers_To(Delivers_To,product_id)
values('Usa','10');





###Colors 
insert into colors(color,product_id)
values('Black','1');
insert into colors(color,product_id)
values('Silver','2');
insert into colors(color,product_id)
values('Red','3');
insert into colors(color,product_id)
values('Golden','4');
insert into colors(color,product_id)
values('Silver','5');
insert into colors(color,product_id)
values('Black','6');
insert into colors(color,product_id)
values('Blue','7');
insert into colors(color,product_id)
values('White','8');
insert into colors(color,product_id)
values('Silver','9');
insert into colors(color,product_id)
values('Black','10');





###ViewedProduct
insert into viewedproduct(customer_id,product_id)
values('11','1');
insert into viewedproduct(customer_id,product_id)
values('12','1');
insert into viewedproduct(customer_id,product_id)
values('13','1');
insert into viewedproduct(customer_id,product_id)
values('14','1');
insert into viewedproduct(customer_id,product_id)
values('15','10');
insert into viewedproduct(customer_id,product_id)
values('16','9');
insert into viewedproduct(customer_id,product_id)
values('17','8');
insert into viewedproduct(customer_id,product_id)
values('18','7');
insert into viewedproduct(customer_id,product_id)
values('19','6');
insert into viewedproduct(customer_id,product_id)
values('20','5');
insert into viewedproduct(customer_id,product_id)
values('11','4');
insert into viewedproduct(customer_id,product_id)
values('12','3');
insert into viewedproduct(customer_id,product_id)
values('13','2');





###Bookmark
insert into bookmark(customer_id,product_id)
values('11','1');
insert into bookmark(customer_id,product_id)
values('12','2');
insert into bookmark(customer_id,product_id)
values('14','5');
insert into bookmark(customer_id,product_id)
values('19','10');
insert into bookmark(customer_id,product_id)
values('15','11');
insert into bookmark(customer_id,product_id)
values('16','12');





###ReviewedProducts
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('11','1','it Was Great I Really Advice Everyone to try it ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('11','2','i Did not Like That Color Alot ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('11','3','I Really Enjoyed it   ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('14','6','it Was Great I Really Advice Everyone to try it They Will Like it ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('12','10','it is So Expensive and Not That Quality ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('15','8','it Was Great ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('16','9','Bad Product Do not Buy it  ');
insert into ReviewedProducts(customer_id,product_id,reviewdescription)
values('12','4','I Loooved it ');

```
