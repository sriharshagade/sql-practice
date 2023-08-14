1.Show unique birth years FROM patients and order them by ascending.

```sql
SELECT distinct Year(birth_date)
FROM patients
ORDER by birth_date;
```
2.Show unique first names FROM the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.
```sql
SELECT first_name
FROM patients
GROUP BY first_name
having count(first_name)=1;
```
3. Show patient_id and first_name FROM patients where their first_name start and ends with 's' and is at least 6 characters long.
```sql
SELECT patient_id,first_name 
FROM patients
where lower(first_name) like 's____%s'
```
4.Show patient_id, first_name, last_name FROM patients who’s diagnosis is 'Dementia'. 

Primary diagnosis is stored in the admissions table.
```sql
SELECT patients.patient_id, first_name, last_name
FROM patients
join admissions on patients.patient_id = admissions.patient_id
WHERE diagnosis = 'Dementia';
```
5.Display every patient's first_name. Order the list by the LENGTH of each name and then by alphabetically
```sql
SELECT first_name 
FROM patients
ORDER BY LENGTH(first_name), first_name;
```
6.Show the total number of male patients and the total number of female patients in the patients table. Display the two results in the same row.
```sql
SELECT (SELECT count(*) FROM patients WHERE GENDER = "M") AS male_count,
       (SELECT count(*) FROM patients WHERE GENDER = "F") AS female_count;
```
7. Show first and last name, allergies FROM patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
```sql
SELECT first_name, last_name, allergies
FROM patients
WHERE allergies IN ('Penicillin' , 'Morphine')
ORDER BY allergies, first_name, last_name;
```
8. Show patient_id, diagnosis FROM admissions. Find patients admitted multiple times for the same diagnosis.

```sql SELECT patient_id, diagnosis
FROM admissions
GROUP BY patient_id, diagnosis
HAVING COUNT(diagnosis = diagnosis) > 1;
```
10. Show the city and the total number of patients in the city. Order FROM most to least patients and then by city name ascending.
```sql
SELECT city, count(*) as num_patients
FROM patients
GROUP BY city
ORDER BY COUNT(*) DESC, city
```
10. Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"
```sql
SELECT first_name, last_name, 'Patient' as role FROM patients
    union all
select first_name, last_name, 'Doctor' from doctors;
```
11.Show all allergies ordered by popularity. Remove NULL values FROM the query.
```sql
SELECT allergies, count(*) aS popularity
FROM patients
WHERE allergies is not null or allergies != 'NKA'
GROUP BY allergies
ORDER BY popularity DESC;
```
12.Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting FROM the earliest birth_date.
```sql
SELECT first_name, last_name, birth_date
FROM patients
WHERE year(birth_date) between 1970 and 1979
ORDER BY birth_date;
```
13.We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in descending order  EX: SMITH,jane
```sql
SELECT concat(upper(last_name),',',lower(first_name)) AS new_name_format
FROM patients
ORDER BY first_name DESC
```
14.Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
``` sql
SELECT province_id, sum(height)
FROM patients
GROUP BY province_id
ORDER BY SUM(height) DESC
LIMIT 3
```
15.Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
```sql
SELECT (max(weight) - min(weight)) as weight_delta
FROM patients
where lower(last_name) = 'maroni'

16.Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
```sql
SELECT  DAY(admission_date) as day_number, count(*)
FROM admissions
GROUP BY DAY(admission_date)
ORDER BY count(*) DESC;
17.Show all columns for patient_id 542's most recent admission_date.
```sql
SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1;
```
18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19. 2. attending_doctor_id contains a 2 and the LENGTH of patient_id is 3 characters.
```sql
SELECT patient_id, attending_doctor_id, diagnosis
FROM admissions
WHERE patient_id % 2 = 1 AND attending_doctor_id IN (1, 5, 19)
   OR (attending_doctor_id LIKE '%2%' AND LENGTH(patient_id) = 3);
```
19.Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.

```sql
SELECT first_name, doctors.last_name, count(*)
FROM doctors
JOIN admissions ON doctors.doctor_id = admissions.attending_doctor_id
GROUP BY first_name, last_name
ORDER BY count(*);
```
20.For each doctor, display their id, full name, and the first and last admission date they attended.
SELECT doctors.doctor_id,
	   concat(first_name,' ', last_name) as full_name, 
       min(admission_date) as first_admission_date, 
       max(admission_date) as last_admission_date
FROM doctors
JOIN admissions on admissions.attending_doctor_id = doctors.doctor_id
GROUP BY doctors.doctor_id
ORDER BY doctors.doctor_id
21. Display the total amount of patients for each province. Order by descending.
```sql
SELECT province_name, count(patient_id)
FROM province_names
join patients on patients.province_id = province_names.province_id
GROUP BY province_name
ORDER BY count(patient_id) DESC;
```
22.For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.
```sql
SELECT concat(patients.first_name, ' ', patients.last_name) as patient_name, diagnosis, concat(doctors.first_name, ' ', doctors.last_name)
FROM patients
JOIN admissions ON patients.patient_id = admissions.patient_id
JOIN doctors ON doctors.doctor_id = admissions.attending_doctor_id
```
23. display the number of duplicate patients based on their first_name and last_name.
```sql
    SELECT patient_id, diagnosis
    FROM admissions
    GROUP BY patient_id, diagnosis
    HAVING COUNT(diagnosis = diagnosis) > 1;
```
25.  Display patient's full name,height in the units feet ROUNDed to 1 decimal,weight in the unit pounds ROUNDed to 0 decimals,birth_date,gender non abbreviated. Convert CM to feet by dividing by 30.48. Convert KG to pounds by multiplying by 2.205.

```sql
SELECT 
	concat(first_name, " ", last_name) as full_name, 
            ROUND((height / 30.48),1), 
            ROUND((weight * 2.205),0),
            birth_date,
    CASE 
    	when gender = 'M' then 'MALE'
            when gender = 'F' then 'FEMALE'
   END as gender
FROM patients
```
25. Show patient_id, first_name, last_name FROM patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)

```sql
SELECT patients.patient_id, first_name, last_name
FROM patients
LEFT JOIN admissions
ON patients.patient_id = admissions.patient_id
WHERE admissions.patient_id IS NULL;
```
