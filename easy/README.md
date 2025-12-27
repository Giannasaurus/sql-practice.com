# Easy
> [!IMPORTANT]
> All questions are completed in this difficulty!

1. Show first name, last name, and gender of patients whose gender is M.
```
SELECT first_name, last_name, gender FROM patients
WHERE gender = 'M'
```

2. Show first name and last name of patients who does not have allergies. (null)
```
SELECT first_name, last_name FROM patients
WHERE allergies IS null
```

3. Show first name of patients that start with the letter 'C'
```
SELECT first_name FROM patients
WHERE first_name LIKE 'C%'
```

4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)
```
SELECT first_name, last_name FROM patients
WHERE weight BETWEEN 100 AND 120
```

5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS null
```

6. Show first name and last name concatinated into one column to show their full name.
```
SELECT
CONCAT(first_name, ' ', last_name) AS full_name
FROM patients
```

7. Show first name, last name, and the full province name of each patient.
Example: 'Ontario' instead of 'ON'
```
SELECT first_name, last_name, PN.province_name FROM patients
JOIN province_names PN ON patients.province_id = PN.province_id
```

8. Show how many patients have a birth_date with 2010 as the birth year.
```
SELECT COUNT(*) as total_patients from patients
WHERE YEAR(birth_date) = 2010
```

9. Show the first_name, last_name, and height of the patient with the greatest height.
```
SELECT first_name, last_name, MAX(height) as height from patients
```

10. Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000
```
SELECT * from patients
WHERE patient_id IN (1, 45, 534, 879, 1000)
```

11. Show the total number of admissions
```
SELECT COUNT(*) as total_admissions FROM admissions
```

12. Show all the columns from admissions where the patient was admitted and discharged on the same day.
```
SELECT * from admissions
WHERE admission_date IS discharge_date
```

13. Show the patient id and the total number of admissions for patient_id 579.
```
SELECT patient_id, COUNT(admission_date) as total_admissions
from admissions
WHERE patient_id IS '579'
```

14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'.
```
SELECT DISTINCT city as unique_cities FROM patients
WHERE province_id IS 'NS'
```

15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70
```
SELECT first_name, last_name, birth_date FROM patients
WHERE height > 160 AND weight > 70
```

16. Write a query to find list of patients first_name, last_name, and allergies where allergies are not null and are from the city of 'Hamilton'
```
SELECT first_name, last_name, allergies FROM patients
WHERE allergies NOT null
AND city IS 'Hamilton'
```