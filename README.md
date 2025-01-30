Description  
-Project-Meet is a meeting room booking system that allows employees to easily select and manage reservations with an   access management system.  

Requirement  
🔹 Backend  
Node.js  
Oracle Database  
🔹 Frontend  
React.js  
Tailwind CSS  


How to install and use  
Set up the database  
-Create a database in Oracle  
-Set up the connection in .env  

install Dependencies  
-npm install  

RUN FRONTEND  
-npm run dev  
RUN BACKEND  
-node server.js  
 

SQL STATEMENT  

CREATE TABLE POSITION (  
  PNUMBER NUMBER(9, 0) NOT NULL,  
  PNAME VARCHAR2(15 BYTE),  
  CONSTRAINT PK_POSITION PRIMARY KEY (PNUMBER)  
);  
CREATE TABLE MENU (  
  MNUMBER NUMBER(9, 0) NOT NULL,  
  MNAME VARCHAR2(30 BYTE),  
  CONSTRAINT PK_MENU PRIMARY KEY (MNUMBER)  
);  
CREATE TABLE BUILDING (  
  BDNUMBER NUMBER(9, 0) NOT NULL,  
  BDNAME VARCHAR2(15),  
  CONSTRAINT PK_BUILDING PRIMARY KEY (BDNUMBER)  
);  
CREATE TABLE DEPARTMENT (  
  DNUMBER NUMBER(9, 0) NOT NULL,  
  DNAME VARCHAR2(30),  
  CONSTRAINT PK_DEPARTMENT PRIMARY KEY (DNUMBER)  
);  
CREATE TABLE FLOOR (  
  FLNUMBER NUMBER(9, 0) NOT NULL,  
  FLNAME VARCHAR2(15),  
  CONSTRAINT PK_FLOOR PRIMARY KEY (FLNUMBER)  
);  
CREATE TABLE MENU (  
  MNUMBER NUMBER(9, 0) NOT NULL,  
  MNAME VARCHAR2(30),  
  CONSTRAINT PK_MENU PRIMARY KEY (MNUMBER)  
);  
CREATE TABLE POSITION (  
  PNUMBER NUMBER(9, 0) NOT NULL,  
  PNAME VARCHAR2(15),  
  CONSTRAINT PK_POSITION PRIMARY KEY (PNUMBER)  
);  
CREATE TABLE SETROOM (  
  BDNUM NUMBER(9, 0) NOT NULL,  
  FLNUM NUMBER(9, 0) NOT NULL,  
  CONSTRAINT SETROOM_PK PRIMARY KEY (BDNUM, FLNUM)  
);  
ALTER TABLE SETROOM  
ADD CONSTRAINT FK_SETROOM_BDNUM FOREIGN KEY (BDNUM) REFERENCES BUILDING(BDNUMBER) ENABLE;  
ALTER TABLE SETROOM  
ADD CONSTRAINT FK_SETROOM_FLNUM FOREIGN KEY (FLNUM) REFERENCES FLOOR(FLNUMBER) ENABLE;  
CREATE TABLE STATUSBOOKING (  
  STATUSBOOKINGID NUMBER(9, 0) NOT NULL,  
  STATUSBOOKINGNAME VARCHAR2(20),  
  CONSTRAINT PK_STATUSBOOKING PRIMARY KEY (STATUSBOOKINGID)  
);  
CREATE TABLE STATUSEMP (  
  STATUSEMPID NUMBER(9, 0) NOT NULL,  
  STATUSEMPNAME VARCHAR2(20),  
  CONSTRAINT PK_STATUSEMPID PRIMARY KEY (STATUSEMPID)  
);  
CREATE TABLE STATUSROOM (  
  STATUSROOMID NUMBER(9, 0) NOT NULL,  
  STATUSROOMNAME VARCHAR2(20),  
  CONSTRAINT PK_STATUSROOM PRIMARY KEY (STATUSROOMID)  
);  
CREATE TABLE ROOMTYPE (  
  RTNUMBER NUMBER(9, 0) NOT NULL,  
  RTNAME VARCHAR2(15),  
  CONSTRAINT PK_ROOMTYPE PRIMARY KEY (RTNUMBER)  
);   
CREATE TABLE ACCESSMENU (  
  NO NUMBER(3, 0) NOT NULL,  
  PNUM NUMBER(9, 0) NOT NULL,  
  MNUM NUMBER(9, 0) NOT NULL,  
  CONSTRAINT ACCESSMENU_PK PRIMARY KEY (PNUM, MNUM, NO),  
  CONSTRAINT ACCESSMENU_FK1 FOREIGN KEY (PNUM) REFERENCES POSITION (PNUMBER),  
  CONSTRAINT ACCESSMENU_FK2 FOREIGN KEY (MNUM) REFERENCES MENU (MNUMBER)  
);  
CREATE TABLE CANCLEROOM (   
  CANCLEID NUMBER(9, 0) NOT NULL,  
  REASON VARCHAR2(100),  
  RESID NUMBER(9, 0),  
  EMPID VARCHAR2(9),  
  CONSTRAINT PK_CANCLEROOM PRIMARY KEY (CANCLEID),  
  CONSTRAINT FK_CANCLE_EMPID FOREIGN KEY (EMPID) REFERENCES EMPLOYEE(SSN),  
  CONSTRAINT FK_CANCLE_RESID FOREIGN KEY (RESID) REFERENCES RESERVE(RESERVERID)  
);  
CREATE TABLE CONTACT (  
  CONTEACTID NUMBER(9, 0) NOT NULL,  
  ESSN VARCHAR2(9),  
  MESSAGE VARCHAR2(100),  
  CONSTRAINT CONTACT_PK PRIMARY KEY (CONTEACTID),  
  CONSTRAINT CONTACT_FK1 FOREIGN KEY (ESSN) REFERENCES EMPLOYEE(SSN)  
);  
CREATE TABLE EMPLOYEE (  
  SSN VARCHAR2(9 BYTE) NOT NULL,  
  FNAME VARCHAR2(15 BYTE),  
  LNAME VARCHAR2(15 BYTE),  
  EMAIL VARCHAR2(40 BYTE),  
  PW VARCHAR2(60 BYTE),  
  DNO NUMBER(9, 0),  
  PNO NUMBER(9, 0),  
  STUEMP NUMBER(9, 0),  
  COUNT NUMBER(9, 0),  
  LOCKCOUNT NUMBER(9, 0),  
  CONSTRAINT EMPLOYEEMN_PK PRIMARY KEY (SSN)  
);   
ALTER TABLE EMPLOYEE  
ADD CONSTRAINT FK_EMP_DEP FOREIGN KEY (DNO) REFERENCES DEPARTMENT(DNUMBER) ENABLE;  
ALTER TABLE EMPLOYEE  
ADD CONSTRAINT FK_EMP_POS FOREIGN KEY (PNO) REFERENCES POSITION(PNUMBER) ENABLE;  
ALTER TABLE EMPLOYEE   
ADD CONSTRAINT FK_EMP_STUEMP FOREIGN KEY (STUEMP) REFERENCES STATUSEMP(STATUSEMPID) ENABLE;  
CREATE TABLE LOCKEMP (  
  LOCKEMPID NUMBER(9, 0) NOT NULL,  
  LOCKDATE DATE,  
  ESSN VARCHAR2(9 BYTE),  
  CONSTRAINT LOCKEMP_PK PRIMARY KEY (LOCKEMPID)  
);  
ALTER TABLE LOCKEMP  
ADD CONSTRAINT FK_LOCK_ESSN FOREIGN KEY (ESSN) REFERENCES EMPLOYEE(SSN) ENABLE;  
CREATE TABLE CONFERENCEROOM (  
  CFRNUMBER NUMBER(9, 0) NOT NULL,  
  CFRNAME VARCHAR2(15),  
  CAPACITY NUMBER(9, 0),  
  RTNUM NUMBER(9, 0),  
  FLNUM NUMBER(9, 0),  
  BDNUM NUMBER(9, 0),  
  STUROOM NUMBER(9, 0),  
  CONSTRAINT PK_CONFERENCEROOM PRIMARY KEY (CFRNUMBER),  
  CONSTRAINT FK_CFR_BUILDING FOREIGN KEY (BDNUM) REFERENCES BUILDING(BDNUMBER),  
  CONSTRAINT FK_CFR_FLOOR FOREIGN KEY (FLNUM) REFERENCES FLOOR(FLNUMBER), 
  CONSTRAINT FK_CFR_ROOMTYPE FOREIGN KEY (RTNUM) REFERENCES ROOMTYPE(RTNUMBER), 
  CONSTRAINT FK_CFR_STUROOM FOREIGN KEY (STUROOM) REFERENCES STATUSROOM(STATUSROOMID)  
);  
CREATE TABLE RESERVE (  
  RESERVERID NUMBER(9, 0) NOT NULL,  
  BDATE DATE,  
  STARTTIME TIMESTAMP(6),  
  ENDTIME TIMESTAMP(6),  
  STUBOOKING NUMBER(9, 0),  
  ESSN VARCHAR2(9),  
  CFRNUM NUMBER(9, 0),  
  QR VARCHAR2(20),  
  TIME TIMESTAMP(6),  
  CONSTRAINT PK_RESERVE PRIMARY KEY (RESERVERID)  
);  
ALTER TABLE RESERVE  
ADD CONSTRAINT FK_RES_CFR FOREIGN KEY (CFRNUM) REFERENCES CONFERENCEROOM(CFRNUMBER) ENABLE;  
ALTER TABLE RESERVE  
ADD CONSTRAINT FK_RES_ESSN FOREIGN KEY (ESSN) REFERENCES EMPLOYEE(SSN) ENABLE;  
ALTER TABLE RESERVE  
ADD CONSTRAINT FK_RES_STUBK FOREIGN KEY (STUBOOKING) REFERENCES STATUSBOOKING(STATUSBOOKINGID) ENABLE;  


