-- Create Database
CREATE DATABASE IF NOT EXISTS school_communication_app;
USE school_communication_app;

-- USERS TABLE (handles login/signup)
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('student','teacher','parent') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- STUDENTS TABLE
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    admission_number VARCHAR(50) UNIQUE,
    class VARCHAR(50),
    date_of_birth DATE,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

-- PARENTS TABLE
CREATE TABLE parents (
    parent_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    phone VARCHAR(20),
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

-- TEACHERS TABLE
CREATE TABLE teachers (
    teacher_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    subject VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

-- PARENT-STUDENT RELATIONSHIP TABLE
CREATE TABLE parent_student (
    id INT AUTO_INCREMENT PRIMARY KEY,
    parent_id INT,
    student_id INT,
    FOREIGN KEY (parent_id) REFERENCES parents(parent_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE
);

-- ANNOUNCEMENTS TABLE
CREATE TABLE announcements (
    announcement_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(200),
    message TEXT,
    created_by INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (created_by) REFERENCES users(user_id)
);

-- STUDENT FOLDERS TABLE
CREATE TABLE student_folders (
    folder_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE
);

-- ASSIGNMENTS TABLE
CREATE TABLE assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(200),
    description TEXT,
    teacher_id INT,
    due_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (teacher_id) REFERENCES teachers(teacher_id)
);

-- STUDENT ASSIGNMENTS TABLE
CREATE TABLE student_assignments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    assignment_id INT,
    submission_file VARCHAR(255),
    submitted_at TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (assignment_id) REFERENCES assignments(assignment_id) ON DELETE CASCADE
);

-- GRADES TABLE
CREATE TABLE grades (
    grade_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    assignment_id INT,
    grade VARCHAR(10),
    remarks TEXT,
    graded_by INT,
    graded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (assignment_id) REFERENCES assignments(assignment_id),
    FOREIGN KEY (graded_by) REFERENCES teachers(teacher_id)
);

-- SAMPLE DATA (OPTIONAL)

INSERT INTO users (full_name,email,password,role) VALUES
('John Teacher','teacher@school.com','123456','teacher'),
('Mary Parent','parent@school.com','123456','parent'),
('David Student','student@school.com','123456','student');

INSERT INTO teachers (user_id,subject) VALUES
(1,'Mathematics');

INSERT INTO students (user_id,admission_number,class,date_of_birth) VALUES
(3,'ADM001','Grade 6','2012-05-10');

INSERT INTO parents (user_id,phone) VALUES
(2,'0700000000');

INSERT INTO parent_student (parent_id,student_id) VALUES
(1,1);

INSERT INTO announcements (title,message,created_by) VALUES
('School Opening','School will reopen on Monday',1),
('Parents Meeting','Parents meeting will be held on Friday',1);

INSERT INTO assignments (title,description,teacher_id,due_date) VALUES
('Math Homework','Solve chapter 5 questions',1,'2026-03-20');

INSERT INTO student_assignments (student_id,assignment_id) VALUES
(1,1);

INSERT INTO grades (student_id,assignment_id,grade,remarks,graded_by) VALUES
(1,1,'A','Excellent work',1);
