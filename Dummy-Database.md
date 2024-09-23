### Just open the Oracle sql command prompt and login to administrator user and copy paste the following codes for creating dummy database:

```SQL
-- Enable Oracle script mode for session (needed for some configurations)
ALTER SESSION SET "_ORACLE_SCRIPT"=TRUE;  

-- Create a new user called 'RESERVATION' with password 'MANAGER'
CREATE USER RESERVATION IDENTIFIED BY MANAGER;

-- Grant DBA privileges to 'RESERVATION' user
GRANT DBA TO RESERVATION;

-- Commit the transaction to save changes
COMMIT;

-- Connect as the 'RESERVATION' user
CONNECT RESERVATION/MANAGER;

-- Create 'CUSTOMER' table to store customer information
CREATE TABLE "RESERVATION"."CUSTOMER" 
(	
    "MAILID" VARCHAR2(40) PRIMARY KEY,  -- Customer's email as the primary key
    "PWORD" VARCHAR2(20) NOT NULL,      -- Password for customer login
    "FNAME" VARCHAR2(20) NOT NULL,      -- First name of the customer
    "LNAME" VARCHAR2(20),               -- Last name of the customer (optional)
    "ADDR" VARCHAR2(100),               -- Customer's address
    "PHNO" NUMBER(12) NOT NULL          -- Customer's phone number
);

-- Create 'ADMIN' table to store admin information
CREATE TABLE "RESERVATION"."ADMIN"
(	
    "MAILID" VARCHAR2(40) PRIMARY KEY,  -- Admin email as the primary key
    "PWORD" VARCHAR2(20) NOT NULL,      -- Admin login password
    "FNAME" VARCHAR2(20) NOT NULL,      -- Admin's first name
    "LNAME" VARCHAR2(20),               -- Admin's last name (optional)
    "ADDR" VARCHAR2(100),               -- Admin's address
    "PHNO" NUMBER(12) NOT NULL          -- Admin's phone number
);

-- Create 'TRAIN' table to store train details
CREATE TABLE "RESERVATION"."TRAIN" 
(	
    "TR_NO" NUMBER(10) PRIMARY KEY,     -- Train number as primary key
    "TR_NAME" VARCHAR2(70) NOT NULL,    -- Train name
    "FROM_STN" VARCHAR2(20) NOT NULL,   -- Departure station
    "TO_STN" VARCHAR2(20) NOT NULL,     -- Destination station
    "SEATS" NUMBER(4) NOT NULL,         -- Available seats on the train
    "FARE" NUMBER(6,2) NOT NULL         -- Ticket fare
);

-- Create 'HISTORY' table to store booking transaction history
CREATE TABLE "RESERVATION"."HISTORY" 
(	
    "TRANSID" VARCHAR2(36) PRIMARY KEY,     -- Unique transaction ID
    "MAILID" VARCHAR2(40) REFERENCES "RESERVATION"."CUSTOMER"(MAILID),  -- Customer's email as foreign key
    "TR_NO" NUMBER(10),                     -- Train number
    "DATE" DATE,                            -- Booking date
    "FROM_STN" VARCHAR2(20) NOT NULL,       -- Departure station
    "TO_STN" VARCHAR2(20) NOT NULL,         -- Arrival station
    "SEATS" NUMBER(3) NOT NULL,             -- Number of seats booked
    "AMOUNT" NUMBER(8,2) NOT NULL           -- Total amount paid for booking
);

-- Commit the changes
COMMIT;

-- Insert admin account data into the 'ADMIN' table
INSERT INTO RESERVATION.ADMIN VALUES('admin@demo.com','admin','System','Admin','Demo Address 123 colony','9874561230');

-- Insert customer data into the 'CUSTOMER' table
INSERT INTO RESERVATION.CUSTOMER VALUES('shashi@demo.com','shashi','Shashi','Raj','Kolkata, West Bengal',954745222);

-- Insert data into the 'TRAIN' table to store available trains and details
INSERT INTO RESERVATION.TRAIN VALUES(10001,'JODHPUR EXP','HOWRAH','JODHPUR', 152, 490.50);
INSERT INTO RESERVATION.TRAIN VALUES(10002,'YAMUNA EXP','GAYA','DELHI', 52, 550.50);
INSERT INTO RESERVATION.TRAIN VALUES(10003,'NILANCHAL EXP','GAYA','HOWRAH', 92, 451);
INSERT INTO RESERVATION.TRAIN VALUES(10004,'JAN SATABDI EXP','RANCHI','PATNA', 182, 550);
INSERT INTO RESERVATION.TRAIN VALUES(10005,'GANGE EXP','MUMBAI','KERALA', 12, 945);
INSERT INTO RESERVATION.TRAIN VALUES(10006,'GARIB RATH EXP','PATNA','DELHI', 1, 1450.75);
INSERT INTO RESERVATION.TRAIN VALUES(10008,'MUMBAI MAIL','HAWRAH','MUMBAI', 100, 2150.75);
INSERT INTO RESERVATION.TRAIN VALUES(10007,'AJMER-SEALDAH EXP','SEALDAH','AJMER', 120, 1000.50);

-- Insert transaction history into 'HISTORY' table for customer 'shashi@demo.com'
INSERT INTO RESERVATION.HISTORY VALUES('BBC374-NSDF-4673','shashi@demo.com',10001,TO_DATE('02-FEB-2024'), 'HOWRAH', 'JODHPUR', 2, 981);
INSERT INTO RESERVATION.HISTORY VALUES('BBC375-NSDF-4675','shashi@demo.com',10004,TO_DATE('12-JAN-2024'), 'RANCHI', 'PATNA', 1, 550);
INSERT INTO RESERVATION.HISTORY VALUES('BBC373-NSDF-4674','shashi@demo.com',10006,TO_DATE('22-JULY-2024'), 'PATNA', 'DELHI', 3, 4352.25);

-- Commit the inserted records
COMMIT;

```
