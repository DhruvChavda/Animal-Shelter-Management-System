#### Customer 
```sql
set search_path to "animal_shelter";
create or replace function fun()
returns trigger
language plpgsql
as $body$
begin
	if(new.id in (select id from "animal_shelter".customer)) then
	raise UNIQUE_VIOLATION using message='id already exist';
	end if;
	
	if(new.email not like '%@%.%') THEN
	RAISE UNIQUE_VIOLATION USING MESSAGE = 'Email
	Not valid ';
	END IF;
	
	if(new.contact_number>9999999999 or new.contact_number<1000000000) then
	RAISE UNIQUE_VIOLATION USING MESSAGE = 'contact number is invalid';
	END IF;
	
	return new;

end
$body$


create trigger t1
before insert 
on animal_shelter.customer
for each row
execute procedure fun();
```

#### Adoption
```sql
set search_path to animal_shelter;
create or replace function fun2()
returns trigger
language plpgsql
as $body$
begin
	if(new.customer_id not in(select id from animal_shelter.customer)) then
	raise UNIQUE_VIOLATION using message='customer id does not exist';
	end if;
	
	if(new.adoption_info like 'animal') then
		if(new.animal_id in(select adoption.animal_id from animal_shelter.adoption )) then
		raise UNIQUE_VIOLATION using message='this animal is already adopted';
		end if;
		
		if(new.shelter_id != (select shelter_id from animal_shelter.animal where id =new.animal_id)) then
		raise UNIQUE_VIOLATION using message='this animal does not exist in this shelter';
		end if;
		
		return new;
		
	else 
		if((select quantity from animal_shelter.pet_accessories where 
		animal_shelter.pet_accessories.shelter_id=new.shelter_id and 
		animal_shelter.pet_accessories.item like new.adoption_info) =0) then
		raise UNIQUE_VIOLATION using message='this item does not exist in this shelter';
		end if;
		
		update animal_shelter.pet_accessories set 
		quantity=quantity-1
		where animal_shelter.pet_accessories.shelter_id=new.shelter_id and 
		animal_shelter.pet_accessories.item like new.adoption_info ;
	end if;
	
	return new;
	
		
	
end
$body$


create trigger t2
before insert 
on animal_shelter.adoption
for each row
execute procedure fun2();
```

#### Donation
```sql
set search_path to animal_shelter;
create or replace function fun3()
returns trigger
language plpgsql
as $body$
begin
	if(new.customer_id not in(select id from animal_shelter.customer)) then
	raise UNIQUE_VIOLATION using message='customer id does not exist';
	end if;
	
	if(new.donation_info like 'animal') then
		if(new.animal_id not in (select id from animal_shelter.animal)) then
		raise UNIQUE_VIOLATION using message='please enter the animal details first';
		end if;
		
		if(new.animal_id in (select animal_id from animal_shelter.donation)) then
		raise UNIQUE_VIOLATION using message='enter valid animal id';
		end if;
		
		if(new.animal_id in (select animal_id from animal_shelter.adoption)) then
		raise UNIQUE_VIOLATION using message='input is invalid: this animal is adopted';
		end if;
		
		if(new.shelter_id != (select shelter_id from animal_shelter.animal where id =new.animal_id)) then
		raise UNIQUE_VIOLATION using message='input is invalid :this animal does not exist in this shelter';
		end if;
		
		
		return new;
	else
		if(new.animal_id is not null) then
		raise UNIQUE_VIOLATION using message='for donation of pet accessories animal id should be NULL';
		end if;
		
		update animal_shelter.pet_accessories set 
		quantity=quantity+1
		where animal_shelter.pet_accessories.shelter_id=new.shelter_id and 
		animal_shelter.pet_accessories.item like new.donation_info ;
		
		return new;
	end if;
end
$body$


create trigger t3
before insert 
on animal_shelter.donation
for each row
execute procedure fun3();
```

#### Animal
```sql
set search_path to animal_shelter;
create or replace function fun4()
returns trigger
language plpgsql
as $body$
begin
	if(new.id in (select id from animal_shelter.animal)) then
	raise UNIQUE_VIOLATION using message='input invalid:animal id already exist';
	end if;
	
	if(new.type not in (select id from animal_shelter.animal_type)) then
	raise UNIQUE_VIOLATION using message='input invalid:animal type does not exist';
	end if;
	
	if(new.shelter_id not in( select id from animal_shelter.shelter)) then
	raise UNIQUE_VIOLATION using message='input invalid:shelter does not exist';
	end if;
	
	if(new.type = 1 ) then
		if(new.breed_type <1 or new.breed_type>6 ) then
		raise UNIQUE_VIOLATION using message='input invalid: animal type and breed are not matching';
		end if;
	end if;
		
	if(new.type = 2) then
		if(new.breed_type <7 or new.breed_type>11 ) then
		raise UNIQUE_VIOLATION using message='input invalid: animal type and breed are not matching';
		end if;
	end if;
	
	if(new.type = 3) then
		if(new.breed_type <12 or new.breed_type>16 ) then
		raise UNIQUE_VIOLATION using message='input invalid: animal type and breed are not matching';
		end if;
	end if;
		
	if(new.type = 4) then
		if(new.breed_type <17 or new.breed_type>21 ) then
		raise UNIQUE_VIOLATION using message='input invalid: animal type and breed are not matching';
		end if;
	end if;
		
	return new;
		
	
end
$body$


create trigger t4
before insert 
on animal_shelter.animal
for each row
execute procedure fun4();
```
