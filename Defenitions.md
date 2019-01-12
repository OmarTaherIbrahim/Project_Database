# Defenitions

## Defenitions Of Entities

### Users 
This is The Main Entity Which Include The Users And all Thier Informations from thier Names , id, banckaccount .. ect 
### Seller
This is Sub Entity Which inherit from Users And Sellers Who Sell the Products on The E-Comerce 
Website

### Customers
This is Sub Entity Which inherit from Users And Those Are The Customers Who Buy The Products From The E-Comerce Website

### Product
in This Entity There Are all the Products That's on The Database And in Which We Disscused All The attributes of the Product 

###  Type
When we talk about Products There’s Must be For Sure a Type for that Product , So in this Entity We discussed They Types Of the Products  

### Section
As Well as Having Types there Must be Sections Which Those Types Refer To So We Made An Entity For that Reason

###  Order 
In This Entity We Discussed The Process When The Customer Order a Product he Can Observe The Reciet No , The Tax , And The Total Price  

###  Delivery Service
This Entity Is Made For Service of Delivering the Product so we can calculate the Price of Delivery As well as The Time Needed .
### Hot
At This realationship We made That The Product Can Be Hot if the Product is Out Less Than 4 months  and it has More than 3.5 Rating ,
### New
And The New Product is A Product that Has Been Out For a less than a Monthand has more than 3.5 Rating 
### BestOfYear
BestOfYear Product is  a Product that Has Been Out For less than a year And it Has A Rating Above 4.

## Relations 
### The Relation between The Customer and Seller “Relationship” 
Which Can Happen Between (N) Customers Can Have a Preferred (M) Sellers Which He Buy From His Product and He Likes The Quality Of His Products so He can Put The Seller From One of his Preferred Seller as Well as it includes The Number of Products The Customers Bought from that Seller and The Rating For That Seller .
### The Relation Between The Customers and Order “Purchase”
In This Realation (1) Customers Can Purchase (N) Orders , Which is Made So The Customer Can Purchase an Order
### The Relation Between The Seller and The Product “Offer”
This Realtionship is Made For The Seller As he can Put Offer  on His Products With a percentage with a Starting date and Finishing date for That Offer 

### The Relation Between The Customer And Product “Review Product ”
After The Customer Order An Order And A The Customer Recives His Product He Can Review The Product So It Can Help The Other Customer’s To Make Choice about Buying Their Products and Contribute Of The Average of The Rating. 
### The Relation Between The Customer and Product “ViewdProduct “
As Well as Having the Option of the Bookmarking the Customer can also Have The Option of Seeing All the Products he Already Just viewd Them So we Made an Entity For that Reason it Includes The Customer id and product id .
### The Relation Between The Customer And Product “Bookmark”
In This Entity We Let The Customer have the Option of Bookmarking  The Product as If he Wished To View it or But The Product Later On He can get Back to it Easily it Includes it has The Product_id as Well The Customer_id for that Purpose 
### The Relation Between The Order And Product  “ Cart “
In This Relation You Can Know The Amount of Products a Customer Ordered in a Single Order 
We Can Have (N) Orders Cart (M) Products 
### The Relation Between The Seller And Product “Sell”
This Relation is When a(1) Seller Can Put (N) Products On The Website 
### The Relation Between Order And Delivery Service “has”
Which In This Relation we can have (N) order “Has” (1) Delievery Services  That Can Deliver That Order  .
### The Relation Between The Product And Type “Serial no”
In This Relation (N) Products can Single Serial Number of Type That’s Why we put it in the Mapping as an attribute 
###The Relation Between The Type And Section “Is in”
In This realation we Can have (N) Types That Have (1) Section that Refer To.
###The ternary relationship Between Product and hot and BestOfYear and New “Is”
This Realation Determines Wither The Prodduct is New Or Hot Or Best Of Year
### The Recursive Realation Between The Section And it’sslef “Parent”
This Realtionship Which Can Have (1) Parent For (N) Child Sections , So we Can Know Every Section For Which Parent They Belong , Example Section Computers Belong To Electronics .

## The Attributes 
### For The User Entity :
__Email__ : The Email of The user 
__UserName__: The Username That The User Has Choosed.
__Password__ : A Password For That User Account.
__Bank Account For The Customer__: The Way The User Will Pay  And As We Create An Account it’s Not Necessary But When The Customer Order it’s Asked To Give The Payment Method  And if He’s A Seller .
__Bank Account For The Seller__ : It’s Neccisry to Put This Infromation So The Seller Can Revice His Payment.
__Phone Number For The Customer__: The Phone Number OF The User So When The Delivery Is Delivered The Delivery Service Can Contact With Him .
__Address__: The Address Of The User So The Delievery Service Can Know Where To Deliever.
__Address For The Seller__: So The Delivery Service Can Come and Take The Seller’s Products.
__Country__ : The Country Of The user is Lving in So This Can help Us in The Deliver To.
__Account Type__ : So We Can Know Wheter The User is A User or A Seller .
__Birthday__ : So The Website can wish him a Happy Birthday .
__User Id__: This Is The Most Important Attribute so we Can Trace This User.

### Seller Entity : 
__Seller id__ : it’s Primary Key for Seller Entity And a Forigen Key Which refers To The User id.
__Rating__ : The Current Rating For that  Seller .

### Customer Entity:
__Customer id__: a Primary Key and a  Forigen Key That Refer For The User Id .

### Product Entity :
__Product id__ : it’s The Primary key For a Product Which Identify The Product .
__Serial no__: this is The Number That Refers For The Type So we can Know The Product Type.
__Price__ : The Price Of That Product That The Seller have Chosen.
__Publish date__: The Date The Product Has Been Published on The web .
__Country__ : The Country The Product That was Made in .
__Owner_id__: it’s a Foreign Key That Reference The Seller id That Own That Product.
__Quantity__ : This so We can Know How Much of This Product we Have .

### Type Entity : 
__Serial_no__: the Number of that Type And it’s Primary key Which Identify That Type
__Type Name__ : So We can Insert The Type name Like ‘Iphone8’
__Section Name__: it’s A Forigen Key That Refer To The Section Name 

### Section Entity:
__Section Name__ : Which refers To The Section Name Like “Computers”.
__ParentSection__ : Which refers To The Parent of A Current Section like “Electronics”.

### Viewed Product:	
__Customer Id__ : Which is a primary key and forigent key refers To The Customer Id .
__Product Id__ : Which is a primary key and forigent key refers to the Product Id .

### Bookmark:
__Customer Id__ : Which is a primary key and forigent key refers To The Customer Id .
__Product Id__ : Which is a primary key and forigent key refers to the Product Id .

### Order:
__Reciet_no__ : which is Primary Key and it’s the Reciet Number of that Order.
__Total Price__ : Total Price Without Tax.
__Total Price With Tax__ : Total Price With Tax.
__Tax__ : The Sum of The Taxes in a Orader .
__Date__ : The Date When the Customer Ordered an Order.
__CustomerIid__ : which referc To The Customer_id  Who Ordered That Order.

### Delievery Service : 
__Reciet_No__ : a Primary Key Which refers to Order Reciet_No So we can Trace This Dilevery For Which Order.
__Time__ :  The Time needed For The Order To Be Delievered .
__Price__ : The Price of The Delievery .

### Cart
__Reciet_No__ : a Primary Key Which refers to Order Reciet_No So we can Trace How Many items a Customer has Bought in a Single Order.
__Product_id__ : So we can Know Which Product did the customer put in The Cart .
__Amount__ : The Amount of The Items That are in The Cart .

### Offer
__Product_id__ : So We can Know On Which Product Did the Seller put His Offer .
__Seller id__ : So We can Know which seller Who Offered This Offer .
__Date Start__ : The Date The Offer Starts .
__Date End__ : The Date The Offer Ends.
__Percentage__ : The Percentage of the Offer on a Product .

### Tags :
__Tag____ :  Tags That can Be Putted on a Product Like “Mobiles”,”iPhone8”.. ect.
__Product_id__ : Primary key That Refer For Which Product we Put Those Tags at .
### Sizes
__Size__ : So We Can Choose A Size for a Current Product like : small or medium or large .. ect;
__Product_id__ : Primary key That Refer For Which Product we Put Those Size to.
### Delivers To :
__Product_id__ : Primary key That Refer For Which Product .
__Country__ : So We can Choose The countries That Seller Choose to Deliever to.
### Colors:
__Product_id__ : Primary key That Refer For Which Product .
__Color__ : So We Can Choose What Color is Available For A Current Product .




