
Shelter(**ID**, name, address, email)  

Animal (**ID**,shelter_id, animal_type, breed_type, age, rate, gender, color , cage_size, height, weight)

Type ( **ID**, name)

Breed (**ID**, name)

Medication (**Breed_ID**, allergies, veterinarian, vaccination_date)

Description ( **Animal_ID**, food_preferences, special_day)

Customer ( **ID**, first_name, last_name, email, address, contact_number)

Adoption ( **Animal_ID**, **Customer_ID**, **date**,  **payment_type**, **adoption_info**,**shelter_id**)

Donation (**Animal_ID**, **Customer_ID**, **donation_info**, **date,shelter_id**)

Staff ( **ID**, shelter_ID, name, position, password, email, Contact_number)

Pet_Accessories (**Shelter_ID**, **item**, quantity, price)