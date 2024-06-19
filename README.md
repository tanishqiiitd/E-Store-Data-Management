# E-Store-Data-Management
 

## Pre-requisite

My-SQL workbench/My-sql command line client

## Contents

- Project Description
- Basic structure
  - Functional requirements
  - Entity Relation (ER) diagram and constraints
  - Relational database schema
- Implementation
  - Creating tables
  - Inserting data
- Queries
  - Basic queries
  - PL/SQL function
  - Trigger function

## 1. Project Description

No seller wishes to fall behind in the current era of online purchasing. In addition, the transition from an offline to an online selling model is growing rapidly because of its ease.Instead,
Therefore, it is our responsibility as engineers to make this transition easier for the seller.
A database system is the most essential component needed by an internet site, among many others. We thus intend to create a database for this project so that small clothes vendors may list their goods online.

## 2. Basic Structure

### 2.1 Functional requirement

- A new user can register on the website.
- A customer can see details of the product present in the cart
- A customer can view his order history.
- Admin can start a sale with certain discount on every product.
- Customer can filter the product based on the product details.
- A customer can add or delete a product from the cart.
- A seller can unregister/ stop selling his product.
- A seller/ customer can update his details.
- Admin can view the products purchased on particular date.
- Admin can view number of products sold on a particular date.
- A customer can view the total price of product present in the cart unpurchased.
- Admin can view details of customer who have not purchased anything.
- Admin can view total profit earned from the website.

### 2.2 Entity Relation Diagram

![ER diagram](https://https://github.com/tanishqiiitd/E-Store-Data-Management/blob/main/pngs/ER.png)

### 2.3 Relational Database Schema

![Relational diagram](https://https://github.com/tanishqiiitd/E-Store-Data-Management/blob/main/pngs/RELATIONAL.png)

## 3. Implementation

You can directly copy and paste all the commands from the text given here into the SQL console to create and insert values into your table.

### 3.1 Creating Tables

```sql
    CREATE TABLE Cart
    (
        Cart_id VARCHAR(7) NOT NULL,
        PRIMARY KEY(Cart_id)
    );

    CREATE TABLE Customer
    (
        Customer_id VARCHAR(6) NOT NULL,
        c_pass VARCHAR(10) NOT NULL,
        Name VARCHAR(20) NOT NULL,
        Address VARCHAR(20) NOT NULL,
        Pincode NUMBER(6) NOT NULL,
        Phone_number_s number(10) NOT NULL,
        PRIMARY KEY (Customer_id),
        Cart_id VARCHAR(7) NOT NULL,
        FOREIGN KEY(Cart_id) REFERENCES cart(Cart_id)
    );

    CREATE TABLE Seller
    (
        Seller_id VARCHAR(6) NOT NULL,
        s_pass VARCHAR(10) NOT NULL,
        Name VARCHAR(20) NOT NULL,
        Address VARCHAR(10) NOT NULL,
        PRIMARY KEY (Seller_id)
    );

    CREATE TABLE Seller_Phone_num
    (
        Phone_num NUMBER(10) NOT NULL,
        Seller_id VARCHAR(6) NOT NULL,
        PRIMARY KEY (Phone_num, Seller_id),
        FOREIGN KEY (Seller_id) REFERENCES Seller(Seller_id)
        ON DELETE CASCADE
    );

    CREATE TABLE Payment
    (
        payment_id VARCHAR(7) NOT NULL,
        payment_date DATE NOT NULL,
        Payment_type VARCHAR(10) NOT NULL,
        Customer_id VARCHAR(6) NOT NULL,
        Cart_id VARCHAR(7) NOT NULL,
        PRIMARY KEY (payment_id),
        FOREIGN KEY (Customer_id) REFERENCES Customer(Customer_id),
        FOREIGN KEY (Cart_id) REFERENCES Cart(Cart_id),
        total_amount numeric(6)
    );

    CREATE TABLE Product
    (
        Product_id VARCHAR(7) NOT NULL,
        Type VARCHAR(7) NOT NULL,
        Color VARCHAR(15) NOT NULL,
        P_Size VARCHAR(2) NOT NULL,
        Gender CHAR(1) NOT NULL,
        Commission NUMBER(2) NOT NULL,
        Cost NUMBER(5) NOT NULL,
        Quantity NUMBER(2) NOT NULL,
        Seller_id VARCHAR(6),
        PRIMARY KEY (Product_id),
        FOREIGN KEY (Seller_id) REFERENCES Seller(Seller_id)
        ON DELETE SET NULL
    );

    CREATE TABLE Cart_item
    (
        Quantity_wished NUMBER(1) NOT NULL,
        Date_Added DATE NOT NULL,
        Cart_id VARCHAR(7) NOT NULL,
        Product_id VARCHAR(7) NOT NULL,
        FOREIGN KEY (Cart_id) REFERENCES Cart(Cart_id),
        FOREIGN KEY (Product_id) REFERENCES Product(Product_id),
        Primary key(Cart_id,Product_id)
    );

    alter table Cart_item add purchased varchar(3) default 'NO';
```

### 3.2 Inserting Values

These are some demo values. Full data will be updated in future commits

```sql
    insert into Cart values('crt1011');
    insert into Cart values('crt1012');
    insert into Cart values('crt1013');
    insert into Cart values('crt1014');
    insert into Cart values('crt1015');

    insert into Customer values('cid100','ABCM1235','rajat','G-453','632014',9893135876, 'crt1011');
    insert into Customer values('cid101','XYZ98765','aman','A-123','110001',9876543210, 'crt1012');
    insert into Customer values('cid102','PASS6789','sneha','B-789','400001',9123456789, 'crt1013');
    insert into Customer values('cid103','HELLO123','nisha','C-567','560001',9345678901, 'crt1014');
    insert into Customer values('cid104','WORLD456','vikas','D-890','700001',9234567890, 'crt1015');


    insert into Seller values('sid100','12345','aman','delhi cmc');
    insert into Seller values('sid101','67890','priya','mumbai abc');
    insert into Seller values('sid102','abcde','vikram','kolkata xyz');
    insert into Seller values('sid103','fghij','sameer','chennai pqr');
    insert into Seller values('sid104','klmno','radhika','bangalore lmn');

    insert into Product values('pid1001','jeans','red','32','M',10,10005,20,'sid100');
    insert into Product values('pid1002','shirt','blue','M','M',12,2000,15,'sid101');
    insert into Product values('pid1003','skirt','green','L','F',8,1500,10,'sid102');
    insert into Product values('pid1004','dress','black','S','F',15,3000,5,'sid103');
    insert into Product values('pid1005','shoes','white','42','M',20,5000,25,'sid104');


    insert into Seller_Phone_num values('9943336206','sid100');
    insert into Seller_Phone_num values('9123456780','sid101');
    insert into Seller_Phone_num values('9345678902','sid102');
    insert into Seller_Phone_num values('9234567891','sid103');
    insert into Seller_Phone_num values('9012345678','sid104');

    insert into Cart_item values(3, to_date('10-OCT-1999','dd-mon-yyyy'),'crt1011','pid1001');
    insert into Cart_item values(2, to_date('11-NOV-1999','dd-mon-yyyy'),'crt1012','pid1002');
    insert into Cart_item values(1, to_date('12-DEC-1999','dd-mon-yyyy'),'crt1013','pid1003');
    insert into Cart_item values(4, to_date('13-JAN-2000','dd-mon-yyyy'),'crt1014','pid1004');
    insert into Cart_item values(5, to_date('14-FEB-2000','dd-mon-yyyy'),'crt1015','pid1005');

    insert into Payment values('pmt1001',to_date('10-OCT-1999','dd-mon-yyyy'),'online','cid100','crt1011',NULL);
    insert into Payment values('pmt1002',to_date('11-NOV-1999','dd-mon-yyyy'),'cash','cid101','crt1012',3000);
    insert into Payment values('pmt1003',to_date('12-DEC-1999','dd-mon-yyyy'),'card','cid102','crt1013',1500);
    insert into Payment values('pmt1004',to_date('13-JAN-2000','dd-mon-yyyy'),'online','cid103','crt1014',5000);
    insert into Payment values('pmt1005',to_date('14-FEB-2000','dd-mon-yyyy'),'cash','cid104','crt1015',1000);
```

## 4. Queries

### 4.1 Basic Queries

#### If the customer wants to see details of product present in the cart

```sql
    select * from product where product_id in(
        select product_id from Cart_item where (Cart_id in (
            select Cart_id from Customer where Customer_id='cid100'
        ))
    and purchased='NO');
```

#### If a customer wants to see order history

```sql
    select product_id,Quantity_wished from Cart_item where (purchased='Y' and Cart_id in (select Cart_id from customer where Customer_id='cid101'));
```

#### Customer wants to see filtered products on the basis of size,gender,type

```sql
    select product_id, color, cost, seller_id from product where (type='jeans' and p_size='32' and gender='F' and quantity>0)
```

#### If customer wants to modify the cart

```sql
    delete from cart_item where (product_id='pid1001' and Cart_id in (select cart_id from Customer where Customer_id='cid100'));
```

#### If a seller stops selling his product

```sql
    delete  from seller where seller_id = 'sid100';
    update product set quantity = 00 where seller_id is NULL;
```

#### If admin want to see what are the product purchased on the particular date

```sql
    select product_id from cart_item where (purchased='Y' and date_added='12-dec-2018');
```
#### How much product sold on the particular date

```sql
    select count(product_id) count_pid,date_added from Cart_item where purchased='Y'  group by(date_added);
```
#### If a customer want to know the total price present in the cart

```sql
    select sum(quantity_wished * cost) total_payable from product p join cart_item c on p.product_id=c.product_id where c.product_id in (select product_id from cart_item where cart_id in(select Cart_id from customer where customer_id='cid101') and purchased=’Y’);
```
#### Show the details of the customer who has not purchased any thing

```sql
    Select * from customer where customer_id not in (select customer_id from Payment);
```
#### Find total profit of the website from sales.

```sql
    select sum(quantity_wished * cost * commission/100) total_profit from product p join cart_item c on p.product_id=c.product_id where purchased=’Y’;
```

### 4.2 PL/SQL function
#### Procedure which returns the type of product with  the cost less than the given cost

```sql
    create or replace procedure cost_filter(c in number,t in varchar)
    is
    cs product.cost%type;
    ty product.type%type;
    id product.product_id%type;
    cursor cf is
    select product_id,cost,type from product where cost<c and type=t;
    begin
    open cf;
    loop
    fetch cf into id,cs,ty;
    exit when cf%notfound;
    dbms_output.put_line('Product' || id || 'has cost ' || cs || ' and the type is' || ty);
    end loop;
    close cf;
    exception
    when no_data_found then
    dbms_output.put_line('Sorry no such products exist');
    end;

```

#### Function which returns total number of products which a particular seller sells

```sql
    create or replace function totalProducts(sId in varchar)
    return number
    is
    total number(2):=0;
    begin
    select count(*) into total
    from product
    where seller_id=sId;
    return total;
    end;
    /
```
Function execution:
```sql
    declare
    c number(2);
    begin
    c:=totalProducts('sid102');
    dbms_output.put_line('Total products is : '|| c);
    end;
```

#### Procedure which returns the total quantity of product with the given ID
Procedure with exception handling
```sql
    create or replace procedure prod_details(p_id in varchar)
    is
    quan number(2);
    begin
    select quantity into quan from product where product_id=p_id;
    exception
    when no_data_found then
    dbms_output.put_line('Sorry no such product exist !!');
    end;
    /
```
### 4.3 Triggers
#### Trigger that will execute before inserting new customer to database and inserting a new cartId to the cart_items table
Function to count number of cart items
```sql
    create or replace function numCartId(cd in varchar)
    return number
    is
    total number(2):=0;
    begin
    select count(*) into total
    from cart_item
    where cart_id=cd;
    return total;
    end;
    Trigger
    Create or replace trigger before_customer
    before insert
    on
    customer
    for each row
    declare
    c varchar(10);
    n number(2);
    begin
    c:= :new.cart_id;
    n:=numCartId(c);
    if n>0 then
    dbms_output.put_line('Sorry');
    end if;
    insert into cart values(c);
    end;
```
#### Trigger to update the total amount of user everytime he adds something to payment table
```sql
    create or replace function total_cost(cId in varchar)
    return number
    is
    total number(2) :=0;
    begin
    select sum(cost) into total from product,cart_item where product.product_id=cart_item.product_id and cart_id=cId;
    return total;
    end;

    create or replace trigger before_pay_up
    before insert
    on
    payment
    for each row
    declare
    total number(3);
    begin
    total :=total_cost(:new.cart_id);
    insert into payment values(:new.payment_id,:new.payment_date,:new.payment_type,:new.customer_id,:new.cart_id,total);
    end;
```

