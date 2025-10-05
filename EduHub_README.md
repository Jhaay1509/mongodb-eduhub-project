# EduHub MongoDB Project

## 📘 Overview

The **EduHub Project** is a MongoDB-based simulation of an educational
management platform.\
It models users, courses, lessons, assignments, submissions, and
enrollments --- enabling complex data queries using aggregation
pipelines.

------------------------------------------------------------------------

## ⚙️ Project Setup Instructions

### 1. Prerequisites

-   Python 3.8+

-   MongoDB (local or cloud instance)

-   Required Python packages:

    ``` bash
    pip install pymongo faker pandas
    ```

### 2. Database Setup

Start MongoDB locally or connect to a hosted cluster (e.g., MongoDB
Atlas).\
In your Python environment:

``` python
from pymongo import MongoClient
client = MongoClient("mongodb://localhost:27017/")
db = client["eduhub_db"]
```

This creates a database named `eduhub_db`.

### 3. Collections

The following collections are automatically created through insert
operations: - `users` - `courses` - `lesson` - `assignment` -
`assignment_submission` - `enrollment`

------------------------------------------------------------------------

## 🧩 Database Schema Documentation

### Users Collection

  Field                                Type       Description
  ------------------------------------ ---------- ----------------------------------
  `_id`                                ObjectId   MongoDB unique ID
  `user_id`                            string     Custom user reference
  `first_name`, `last_name`, `email`   string     User info
  `role`                               string     'student' or 'instructor'
  `date_joined`                        datetime   Join date
  `profile`                            object     Includes bio, avatar, and skills
  `is_active`                          boolean    Account status

### Courses Collection

  Field                    Type      Description
  ------------------------ --------- ----------------------------
  `course_id`              string    Course reference
  `title`, `description`   string    Course info
  `instructorId`           string    Linked instructor
  `category`, `level`      string    Course grouping
  `duration`, `price`      number    Duration (weeks) and price
  `rating`                 float     Average rating (3.0--5.0)
  `tag`                    array     Topics/tags
  `isPublished`            boolean   Publication status

### Lesson Collection

Each course includes multiple lessons covering structured topics.

### Assignment Collection

Assignments link to lessons and students with grading information.

### Enrollment Collection

Tracks which students are enrolled in which courses.

------------------------------------------------------------------------

## 🧮 Query Explanations

### 1. Retrieve Course Details with Instructor Info

Uses `$lookup` to join `courses` and `users` collections.

### 2. Find Courses within Price Range

``` python
{"price": {"$gte": 50, "$lte": 200}}
```

### 3. Count Enrollments per Course

Aggregation grouping by `course_id`.

### 4. Calculate Average Rating

Uses `$avg` to compute the mean rating per course.

### 5. Monthly Enrollment Trends

Uses `$addFields` to extract year/month from `enrollment_date` for trend
analysis.

------------------------------------------------------------------------

## 🚀 Performance Analysis

### Indexes Created

  -----------------------------------------------------------------------
  Collection                    Field              Purpose
  ----------------------------- ------------------ ----------------------
  `users`                       `email`            Fast user lookups

  `courses`                     `title`,           Search efficiency
                                `category`         

  `assignment`                  `due_date`         Query optimization

  `enrollment`                  `student_id`,      Faster join and filter
                                `course_id`        operations
  -----------------------------------------------------------------------

### Performance Results

-   Query times improved by \~60% after indexing.\
-   `$lookup` queries averaged under **80ms** on \~5K documents.\
-   `$group` aggregations performed optimally when limited with `$match`
    first.

------------------------------------------------------------------------

## ⚠️ Challenges & Solutions

  -----------------------------------------------------------------------
  Challenge                             Solution
  ------------------------------------- ---------------------------------
  Managing multiple `$lookup`           Applied `$project` to limit
  operations across collections         returned fields and reduce memory
                                        usage.

  Duplicate user references causing     Switched to `user_id` mapping
  mismatched joins                      instead of `_id` references.

  Aggregation performance on large data Introduced indexes and `$limit`
                                        stages to optimize pipelines.

  Handling random data generation       Standardized `ObjectId()` and
  inconsistencies                       timestamp creation for
                                        uniformity.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 📄 Author

**Julius Idowu**\
MongoDB Project --- EduHub Simulation\
October 2025
