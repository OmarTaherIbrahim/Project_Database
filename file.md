
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
# insert statements 
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



## drop statements

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