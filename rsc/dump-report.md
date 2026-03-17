-- =========================================================
-- Questions:
-- =========================================================

-- 1. List all students

select * from students

-- 2. Show students and their departments

select s.student_number, s.full_name, d.name
from students s inner join departments d on s.department_id = d.id

-- 3. Count students by status

select status, count(*)
from students
GROUP BY status

-- 4. List courses with department name

select c.code, c.title, d.name as department
from courses c
inner join departments d on d.id = c.department_id

-- 5. Show enrollments with student and course

select s.student_number, s.full_name, c.code, c.title, e.final_status
from enrollments e
inner join students s ON e.student_id = s.id
inner join courses c on c.id = e.offering_id

-- 6. Average grade by course offering

SELECT c.code, c.title, round(sum(g.grade)/count(g.grade), 2) as average
from grades g
inner join assignments a on a.id = g.assignment_id
inner join course_offerings co on co.id = a.offering_id
inner join courses c on c.id = co.course_id
GROUP BY c.id

-- 7. Students with no grades yet

SELECT s.student_number, s.full_name, g.grade
from grades g
left join students s on s.id = g.student_id
WHERE g.grade is null

-- 8. Courses with more than 2 enrolled students

SELECT c.code, c.title, count(*)
from courses c
inner join departments d on d.id = c.department_id
inner join students s on s.department_id = d.id
GROUP BY c.id
having count(*) > 2

OU

SELECT c.code, c.title, count(*) as students
from enrollments e
inner join students s on s.id = e.student_id
inner join course_offerings co on co.id = e.offering_id
inner join courses c on c.id = co.course_id
where e.final_status = 'enrolled'
group by c.id
having count(*) > 2