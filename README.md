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
create table Section(
    SectionName varchar(15) not null,
    ParentSection varchar(15) not null,
    constraint Section_pk PRIMARY key(SectionName),
    constraint Section_Parent_fk foreign key (ParentSection) references Section(SectionName),
    unique(SectionName)
);
create table Type(
    Serial_no int auto_increment primary key,
    TypeName varchar(15) not null unique,
    SectionName varchar(15)not null,
    foreign key(SectionName) references Section(SectionName) on delete cascade
);
create table Product(
    Product_ID int auto_increment primary key,
    serial_no int ,
    foreign key(serial_no) references Type(Serial_no),
    Price int not null,
    PublishDate Date not null,
    Country varchar(20) not null,
    Owner_ID int not null,
    Quantity int not null check (Quantity>=0)
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
    birthdate date, # 8 because like 01-01-1998
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
    references Account_user(userid)
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
    constraint sharview_PK primary key(customer_email,second_customer_email),
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
    constraint  foreign key (reciet_no) references customer(customer_id)
);
		
create table Delivery_service(
    reciet_no int not null ,
    Deleivery_time date ,
    price float ,
    constraint Delivery_Check check(price > 0),
    constraint Deleivery_PK primary key (reciet_no),
    constraint Delivery_Fk foreign key (reciet_no) references customer_order(reciet_no)
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

```
## Views
```SQl
create view Hot as
select Product_ID as HotProduct from Product where DATEDIFF(current_date,product.PublishDate)<120;
create view New as
select Product_ID as NewProduct from Product where DATEDIFF(current_date,product.PublishDate)<30;
create view BestOfYear as
select Product_ID as BestOfYearProduct from Product where DATEDIFF(current_date,product.PublishDate)<365;
