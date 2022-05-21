--SOAL NO 1
SELECT CONCAT(B.first_name,' ',B.last_name) AS FULLNAME, P.NAME AS JABATAN, DATEDIFF(YEAR, B.DOB, GETDATE()) AS USIA, COUNT(F.status) AS JUMLAHANAK
FROM biodata AS B
JOIN employee AS E ON E.biodata_id = B.id
JOIN employee_position AS EP ON EP.employee_id = E.id
JOIN position AS P ON P.id = EP.position_id	
JOIN family AS F ON F.biodata_id = B.id
WHERE F.status = 'Anak'
GROUP BY B.first_name, B.last_name, P.name, B.dob, F.status

--SOAL NO 2
SELECT CONCAT(b.first_name,' ',b.last_name) AS FullName, SUM(DATEDIFF(DAY, lr.start_date,lr.end_date)) AS Cuti
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN leave_request AS lr ON lr.employee_id = e.id
GROUP BY b.first_name, b.last_name

--SOAL NO 3
SELECT TOP 3 CONCAT(B.first_name,' ',B.last_name) AS FULLNAME, P.NAME AS JABATAN, DATEDIFF(YEAR, B.DOB, GETDATE()) AS USIA
FROM biodata AS B
JOIN employee AS E ON E.biodata_id = B.id
JOIN employee_position AS EP ON EP.employee_id = E.id
JOIN position AS P ON P.id = EP.position_id	
ORDER BY B.dob ASC

--SOAL NO 4
SELECT id, CONCAT(first_name,' ', last_name) AS FullName, pob, address
FROM biodata
WHERE id NOT IN (SELECT biodata_id FROM employee)

--SOAL NO 5
SELECT p.name AS Jabatan, AVG(e.salary) AS Average_Salary
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN employee_position AS ep ON ep.employee_id = e.id
JOIN position AS p ON p.id = ep.position_id
WHERE p.name = 'Staff'
GROUP BY p.name

--SOAL NO 6 
SELECT CONCAT(b.first_name,' ',b.last_name) AS FullName, tt.name AS TypeName, tr.start_date, SUM(ts.item_cost) AS Cost
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN travel_request AS tr ON tr.employee_id = e.id
JOIN travel_type AS tt ON tt.id = tr.travel_type_id
JOIN travel_settlement AS ts ON ts.travel_request_id = tr.id
GROUP BY b.first_name, b.last_name, tt.name, tr.start_date

--SOAL NO 7
SELECT CONCAT(b.first_name,' ',b.last_name) AS FullName, el.regular_quota - SUM(DATEDIFF(DAY, lr.start_date,lr.end_date)) AS Cuti
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN employee_leave AS el ON el.employee_id = e.id
JOIN leave_request AS lr ON lr.employee_id = e.id
WHERE el.period = 2020
GROUP BY b.first_name, b.last_name, el.regular_quota

--SOAL NO 8
SELECT AVG(e.salary) AS Average_Salary
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN employee_position AS ep ON ep.employee_id = e.id
JOIN position AS p ON p.id = ep.position_id
WHERE NOT p.name = 'Staff'

--SOAL NO 9
SELECT 
CASE
	WHEN marital_status = 1 THEN 'Menikah'
	ELSE 'Tidak Menikah' 
END AS MartialStatus, COUNT(marital_status) AS Total
FROM biodata
GROUP BY marital_status

--SOAL NO 10
SELECT CONCAT(b.first_name,' ',b.last_name) AS FullName, SUM(DATEDIFF(DAY, lr.start_date,lr.end_date)) +
(SELECT  SUM(DATEDIFF(DAY, tr.start_date, tr.end_date))
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN travel_request AS tr ON tr.employee_id = e.id
WHERE b.first_name = 'Raya') AS LeaveAndTravel
FROM biodata AS b
JOIN employee AS e ON e.biodata_id = b.id
JOIN leave_request AS lr ON lr.employee_id = e.id
WHERE b.first_name = 'Raya'
GROUP BY b.first_name, b.last_name