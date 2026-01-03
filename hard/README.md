## Hard

1. Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.
For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.
```
SELECT
 COUNT(*) AS patients_in_group,
    FLOOR(weight / 10) * 10 AS weight_group
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC
```

2. Show patient_id, weight, height, isObese from the patients table.
Display isObese as a boolean 0 or 1.
Obese is defined as weight(kg)/(height(m)2) >= 30.
weight is in units kg.
height is in units cm.
```
SELECT patient_id, weight, height,
(CASE
 WHEN weight/(POWER(height/100.0, 2)) >= 30 THEN 1
 ELSE 0
end) AS isObese
FROM patients
```

3. Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'
Check patients, admissions, and doctors tables for required information.
```
SELECT
 p.patient_id,
    p.first_name AS patient_first_name,
    p.last_name AS patient_last_name,
    d.specialty AS attending_doctor_specialty
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.attending_doctor_id = d.doctor_id
WHERE a.diagnosis = 'Epilepsy' AND d.first_name = 'Lisa'
```

4. All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.
The password must be the following, in order:
- patient_id
- the numerical length of patient's last_name
- year of patient's birth_date
```
SELECT DISTINCT p.patient_id, 
 CONCAT(p.patient_id, LEN(p.last_name), YEAR(p.birth_date)) AS temp_password
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
```

5. Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.
Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.
```
SELECT
(CASE
 WHEN patient_id % 2 = 0 THEN 'Yes'
  ELSE 'No'
END) AS has_insurance,
SUM(CASE
 WHEN patient_id % 2 = 0 THEN 10
  ELSE 50
END) AS cost_after_insurance
FROM admissions
GROUP BY has_insurance
```