#### Animals
```sql
CREATE TABLE animal
( 
	ID integer primary key, 
	shelter_id integer references shelter(id) ON DELETE CASCADE ON UPDATE CASCADE,
	type integer references animal_type(id) ON DELETE CASCADE ON UPDATE CASCADE,
	breed_type integer references breed(id) ON DELETE CASCADE ON UPDATE CASCADE,
	age integer,
	rate integer,
	gender varchar(10),
	colour varchar(15),
	cage_size varchar(2),
	height float,
	weight float
);
```

#### Shelter
```sql
CREATE TABLE shelter
( 
	id int primary key,
	name varchar(255) not null,
	address varchar(255),
	email varchar(150)
);
```

#### Pet Accessories
```sql
CREATE TABLE Pet_Accessories
( 
	shelter_id integer references shelter(id) ON DELETE CASCADE ON UPDATE CASCADE,
	item varchar(100),
	quantity bigint,
	price bigint
);
```

#### Staff
```sql
CREATE TABLE staff
( 
	id integer primary key,
	shelter_id integer references shelter(id) ON DELETE CASCADE ON UPDATE CASCADE,
	name varchar(100) not null,
	position varchar(50),
	password varchar(15),
	email varchar(50),
	contact_number bigint
);
```

#### Donation
```sql
CREATE TABLE donation
( 
	animal_id integer references animal(id) ON DELETE CASCADE ON UPDATE CASCADE,
	customer_id integer references customer(id) ON DELETE CASCADE ON UPDATE CASCADE,
	donation_info varchar(150),
	date varchar(100)	
);
```


#### Description
```sql
CREATE TABLE description
( 
	
	animal_id integer references animal(id) ON DELETE CASCADE ON UPDATE CASCADE,
	food_preferences varchar(25),
	special_day varchar(100)	
);
```

#### Breed
```sql
CREATE TABLE breed
( 
	id integer primary key,
	name varchar(100) not null
);
```

#### Animal Type
```sql
CREATE TABLE animal_type
( 
	id integer primary key,
	name varchar(100) not null
);
```

#### Customer
```sql
Create Table customer
(
	ID integer Primary Key,
	first_name varchar(255) NOT NULL,
	last_name varchar(255) NOT NULL,
	email varchar(100),
	Address varchar(255),
	Contact_number bigint	
);
```

#### Medication
```sql
Create Table medication
(
	breed_id integer references breed(id) ON DELETE CASCADE ON UPDATE CASCADE,
	allergies varchar(100),
	veterinarian varchar(100),
	vaccination_date varchar(50)
);
```

#### Adoption
```sql
Create Table adoption
(
	animal_id integer references animal(id) ON DELETE CASCADE ON UPDATE CASCADE,
	customer_id integer references customer(id) ON DELETE CASCADE ON UPDATE CASCADE,
	date varchar(50),
	payment_mode varchar(10)
);
```


