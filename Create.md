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