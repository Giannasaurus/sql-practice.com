# Medium
> [!IMPORTANT]
> All questions are completed for this difficulty!

1. Show unique birth years from patients and order them by ascending.
```
SELECT DISTINCT YEAR(birth_date) FROM patients
 ORDER BY YEAR(birth_date) ASC
```

2. Show unique first names from the patients table which only occurs once in the list.
For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.
```
SELECT first_name FROM patients
 GROUP BY first_name
 HAVING COUNT(first_name) is 1
```

3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.
```
SELECT patient_id, first_name FROM patients
WHERE first_name
LIKE "S____%s"
```

4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.
Primary diagnosis is stored in the admissions table.
```
SELECT p.patient_id, first_name, last_name
FROM patients as p
RIGHT JOIN admissions a ON p.patient_id = a.patient_id
WHERE diagnosis = 'Dementia'
```

5. Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.
```
SELECT first_name FROM patients
ORDER BY
 LEN(first_name),
 first_name
```

6. Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.
```
SELECT 
  SUM(gender = 'M') as male_count, 
  SUM(gender = 'F') AS female_count
FROM patients
```

7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
```
SELECT first_name, last_name, allergies FROM patients
WHERE allergies = 'Penicillin' OR allergies = 'Morphine'
ORDER BY allergies ASC, first_name, last_name
```

8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
```
SELECT patient_id, diagnosis FROM admissions
GROUP BY patient_id, diagnosis
HAVING COUNT(admission_date) > 1
```

9. Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.
```
SELECT city,
COUNT(*) as num_patients
FROM patients
GROUP BY city
ORDER BY num_patients DESC, city
```

10. Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"
```
SELECT first_name, last_name, 'Patient' as role from patients
UNION ALL 
SELECT first_name, last_name, 'Doctor' as role FROM doctors
```

11. Show all allergies ordered by popularity. Remove NULL values from query.
```
SELECT allergies, COUNT(allergies) as total_diagnosis 
FROM patients
WHERE allergies NOT null
GROUP BY allergies
ORDER BY total_diagnosis DESC
```

12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.
```
SELECT first_name, last_name, birth_date FROM patients
WHERE YEAR(birth_date) BETWEEN 1970 AND 1979
order BY birth_date
```

13. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane
```
SELECT CONCAT(UPPER(last_name), ",", LOWER(first_name))
AS new_name_format
FROM patients
ORDER BY first_name DESC
```

14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
```
SELECT province_id, SUM(height) as sum_height
FROM patients
GROUP BY province_id
HAVING sum_height >= 7000
```

15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
```
SELECT (MAX(weight) - MIN(weight)) as weight_delta
FROM patients
WHERE last_name IS "Maroni"
```

16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
```
SELECT day(admission_date) as day_number, COUNT(admission_date) as number_of_admissions
FROM admissions
GROUP by day_number
ORDER BY number_of_admissions DESC
```

17. Show all columns for patient_id 542's most recent admission_date.
```
SELECT * FROM admissions
WHERE patient_id IS 542
group by patient_id
HAVING MAX(admission_date)
```

18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.
```
SELECT patient_id, attending_doctor_id, diagnosis FROM admissions
WHERE (patient_id % 2 != 0 AND attending_doctor_id IN (1, 5, 19))
or (attending_doctor_id LIKE '%2%' AND len(patient_id) IS 3)
```

19. Show first_name, last_name, and the total number of admissions attended for each doctor.
Every admission has been attended by a doctor.
```
SELECT first_name, last_name, COUNT(*) as admissions_total FROM doctors
JOIN admissions ON doctors.doctor_id = admissions.attending_doctor_id
GROUP BY doctor_id
```

20. For each doctor, display their id, full name, and the first and last admission date they attended.
```
SELECT doctor_id, CONCAT(first_name, " ", last_name) as full_name,
MIN(admission_date) as first_admission_date, MAX(admission_date) as last_admission_date
from doctors
JOIN admissions ON doctors.doctor_id = admissions.attending_doctor_id
GROUP BY doctor_id
```

21. Display the total amount of patients for each province. Order by descending.
```
SELECT province_name, COUNT(*) as patient_count from patients
JOIN province_names ON patients.province_id = province_names.province_id
GROUP BY province_name 
ORDER BY patient_count DESC
```

22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.
```
SELECT CONCAT(p.first_name, " ", p.last_name) as patient_name,
diagnosis,
CONCAT(d.first_name, " ", d.last_name) as doctor_name
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.attending_doctor_id = d.doctor_id
```

23. display the first name, last name and number of duplicate patients based on their first name and last name.
Ex: A patient with an identical name can be considered a duplicate.
```
SELECT first_name, last_name, COUNT(*) as num_of_duplicates FROM patients
GROUP BY first_name, last_name
HAVING num_of_duplicates IS 2
```

24. Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.
Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.
```
SELECT CONCAT(first_name, " ", last_name) as 'patient_name',
ROUND(height / 30.48, 1) as 'height "Feet"', 
ROUND(weight * 2.205, 0) as 'weight "Pounds"',
birth_date,
CASE
WHEN gender = 'M' THEN 'MALE' 
ELSE 'FEMALE'
END as 'gender_type'
FROM patients
```

25. Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)
```
SELECT patients.patient_id, first_name, last_name FROM patients
WHERE patients.patient_id
NOT IN (SELECT admissions.patient_id FROM admissions)
```

26. Display a single row with max_visits, min_visits, average_visits where the maximum, minimum and average number of admissions per day is calculated. Average is rounded to 2 decimal places.
```
SELECT MAX(number_of_visits) AS max_visits,
MIN(number_of_visits) AS min_visits,
ROUND(AVG(number_of_visits), 2) AS average_visits
FROM (SELECT admission_date, COUNT(*) AS number_of_visits
      FROM admissions
      GROUP BY admission_date)
```

27. Display every patient that has at least one admission and show their most recent admission along with the patient and doctor's full name.
```
SELECT
CONCAT(p.first_name, " ", p.last_name) AS patient_name,
a.admission_date,
CONCAT(d.first_name, " ", d.last_name) AS doctor_name
FROM patients p 
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.attending_doctor_id = d.doctor_id
WHERE a.admission_date = (
  SELECT MAX(a2.admission_date)
  FROM admissions a2
  WHERE a2.patient_id = p.patient_id)
```

