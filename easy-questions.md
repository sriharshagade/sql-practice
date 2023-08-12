## www.sql-practise.com

I found (https://www.sql-practice.com) through some friends, tried to solve all problems present in them.


The test contains 50 questions and consists of 3 category:

- Easy
- Medium
- Hard

In this page I would love to share my approch to solve all easy level problems 

You can use the following Entity Relationship Diagram (ERD) to figure out what data to store, the entities, their attributes and also how entities 
relate to other entities.

![erd](https://user-images.githubusercontent.com/19313466/211203253-23f70de3-4786-45aa-8d7f-54fb81525415.png)

## Answers

---

### Section1: Easy

---

Questions 1- 17

1. Show first name, last name, and gender of patients who's gender is 'M'

```sql
SELECT first_name,
       last_name,
       gender
FROM patients
WHERE gender = 'M';
```

2. Show first name and last name of patients who does not have allergies. (null)

 ```sql
SELECT first_name,
       last_name
FROM patients
WHERE allergies IS NULL
```

3. Show first name of patients that start with the letter 'C'

```sql
SELECT first_name
FROM patients
WHERE first_name LIKE 'C%';
```

4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

```sql
SELECT first_name, last_name
FROM patients
WHERE weight BETWEEN 100 AND 120
```

5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

```sql
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

6. Show first name and last name concatenated into one column to show their full name.

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM patients;
```

7. Show first name, last name, and the full province name of each patient.

```sql
SELECT first_name, last_name, province_name
FROM patients
         JOIN province_names ON patients.province_id = province_names.province_id;
```

8. Show how many patients have a birth_date with 2010 as the birth year.

```sql

SELECT COUNT(birth_date)
FROM patients
WHERE birth_date LIKE '2010%';

```

9. Show the first_name, last_name, and height of the patient with the greatest height.

```sql
SELECT first_name, last_name, MAX(height) AS height
FROM patients
GROUP BY first_name, last_name
ORDER BY height DESC
LIMIT 1;
```

10. Show all columns for patients who have one of the following patient_ids:
    1,45,534,879,1000

```sql

SELECT *
FROM patients
WHERE patient_id IN (1, 45, 534, 879, 1000);

11. Show the total number of admissions

```sql
SELECT COUNT(admissions.patient_id) AS total_number_of_admissions
FROM admissions;
```

12. Show all the columns from admissions where the patient was admitted and discharged on the same day.

```sql
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

13. Show the total number of admissions for patient_id 579.

```sql
SELECT patient_id, COUNT(*) AS total_admissions
FROM admissions
WHERE patient_id = 1
GROUP BY patient_id;
```

14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

```sql
SELECT DISTINCT(city) AS unique_cities
FROM patients
         JOIN province_names ON patients.province_id = province_names.province_id
WHERE province_names.province_id = 'NS';
```

15. Write a query to find the first_name, last name and birth date of patients who have height more than 160 and weight more than 70

```sql
SELECT first_name, last_name, birth_date
FROM patients
WHERE height > 160
  AND weight > 70;
```

16. Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null

```sql
SELECT first_name, last_name, allergies
FROM patients
WHERE city = 'Hamilton'
  AND allergies IS NOT NULL;
```

17. Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the
    result order in ascending by city.

```sql
SELECT city
FROM patients
   OR lower(city) LIKE 'a%'
   OR lower(city) LIKE 'e%'
   OR lower(city) LIKE 'i%'
   OR lower(city) LIKE 'o%'
   OR lower(city) LIKE 'u%'
ORDER BY city ASC;
```

---
