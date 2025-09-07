# database
CREATE TABLE Students (
    student_id INTEGER PRIMARY KEY,
    name TEXT,
    college_id INTEGER,
    email TEXT
);

CREATE TABLE Events (
    event_id INTEGER PRIMARY KEY,
    name TEXT,
    type TEXT,
    date DATE,
    college_id INTEGER
);

CREATE TABLE Registrations (
    reg_id INTEGER PRIMARY KEY,
    student_id INTEGER,
    event_id INTEGER
);

CREATE TABLE Attendance (
    attendance_id INTEGER PRIMARY KEY,
    reg_id INTEGER,
    status TEXT
);

CREATE TABLE Feedback (
    feedback_id INTEGER PRIMARY KEY,
    reg_id INTEGER,
    rating INTEGER
);
INSERT INTO Students VALUES (1, 'Alice', 101, 'alice@college.com');
INSERT INTO Students VALUES (2, 'Bob', 101, 'bob@college.com');

INSERT INTO Events VALUES (1, 'Hackathon', 'Workshop', '2025-09-20', 101);
INSERT INTO Events VALUES (2, 'Tech Talk', 'Seminar', '2025-09-25', 101);

INSERT INTO Registrations VALUES (1, 1, 1);
INSERT INTO Registrations VALUES (2, 2, 1);
INSERT INTO Registrations VALUES (3, 1, 2);

INSERT INTO Attendance VALUES (1, 1, 'Present');
INSERT INTO Attendance VALUES (2, 2, 'Absent');

INSERT INTO Feedback VALUES (1, 1, 5);
-- Event Popularity
SELECT e.name, COUNT(r.reg_id) AS total_registrations
FROM Events e
LEFT JOIN Registrations r ON e.event_id = r.event_id
GROUP BY e.name
ORDER BY total_registrations DESC;

-- Student Participation
SELECT s.name, COUNT(a.attendance_id) AS attended_events
FROM Students s
JOIN Registrations r ON s.student_id = r.student_id
JOIN Attendance a ON r.reg_id = a.reg_id
WHERE a.status = 'Present'
GROUP BY s.name;

-- Top 3 Active Students
SELECT s.name, COUNT(r.reg_id) AS total_events
FROM Students s
JOIN Registrations r ON s.student_id = r.student_id
GROUP BY s.name
ORDER BY total_events DESC
LIMIT 3;

INSERT INTO Feedback VALUES (2, 3, 4);
