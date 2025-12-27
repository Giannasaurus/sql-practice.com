# Medium

Show unique birth years from patients and order them by ascending.
```
SELECT DISTINCT YEAR(birth_date) FROM patients
 ORDER BY YEAR(birth_date) ASC
```

Show unique first names from the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.
```
SELECT first_name FROM patients
 GROUP BY first_name
 HAVING COUNT(first_name) is 1
```

Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.
```
SELECT patient_id, first_name FROM patients
WHERE first_name
LIKE "S____%s"
```

Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.

Primary diagnosis is stored in the admissions table.
```
SELECT p.patient_id, first_name, last_name
FROM patients as p
RIGHT JOIN admissions a ON p.patient_id = a.patient_id
WHERE diagnosis = 'Dementia'
```

Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.
```
SELECT first_name FROM patients
ORDER BY
 LEN(first_name),
 first_name
```

Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.
```
SELECT 
  SUM(gender = 'M') as male_count, 
  SUM(gender = 'F') AS female_count
FROM patients
```

Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
```
SELECT first_name, last_name, allergies FROM patients
WHERE allergies = 'Penicillin' OR allergies = 'Morphine'
ORDER BY allergies ASC, first_name, last_name
```

Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
```
SELECT patient_id, diagnosis FROM admissions
GROUP BY patient_id, diagnosis
HAVING COUNT(admission_date) > 1
```

Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.
```
SELECT city,
COUNT(*) as num_patients
FROM patients
GROUP BY city
ORDER BY num_patients DESC, city
```

Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"
```
SELECT first_name, last_name, 'Patient' as role from patients
UNION ALL 
SELECT first_name, last_name, 'Doctor' as role FROM doctors
```