# Exercise — Salesperson Manager Console App

## Overview

You're going to build a console application that manages salespeople in the `qastore` database. This exercise brings together everything you've covered: classes, instances, methods, and JDBC.

The database already has the data you need — no setup script required. The tables you'll be working with are:

- `salesperson` — the main table you'll do all four CRUD operations on
- `dept` — you can use this to look up department names if you want a challenge

Your finished application will look like this:

```
=== Salesperson Manager ===
1. View all salespeople
2. Add a salesperson
3. Update a salary
4. Delete a salesperson
5. Exit
Choose an option:
```

---

## Your Task

Build three classes in your existing `jdbc1` Maven project:

```
src/main/java/com/qa/
    Salesperson.java
    SalespersonManager.java
    App.java
```

---

### Class 1 — `Salesperson.java`

Represents a single salesperson. Fields to include:

| Field | Type | Column in DB |
|-------|------|--------------|
| empNo | int | emp_no |
| firstName | String | first_name |
| lastName | String | last_name |
| deptNo | int | dept_no |
| salary | double | salary |
| county | String | county |

You need:
- A constructor that takes all six fields
- Getters for all six fields
- A `toString()` that prints neatly, for example:
```
[10] Ferne Filmore | Dept: 1 | Salary: £10.00 | Surrey
```

---

### Class 2 — `SalespersonManager.java`

Handles all the database operations. Four methods:

**`getAllSalespeople()`**
Returns a `List<Salesperson>` of every salesperson.
- Query: `SELECT * FROM salesperson`
- Create a `Salesperson` object per row, add to list, return it

**`addSalesperson(int empNo, String firstName, String lastName, int deptNo, double salary, String county)`**
Inserts a new salesperson.
- Use `PreparedStatement`
- Query: `INSERT INTO salesperson (emp_no, first_name, last_name, dept_no, salary, county) VALUES (?, ?, ?, ?, ?, ?)`
- Print "Salesperson added!" if successful

**`updateSalary(int empNo, double newSalary)`**
Updates a salesperson's salary.
- Use `PreparedStatement`
- Query: `UPDATE salesperson SET salary = ? WHERE emp_no = ?`
- Print "Salary updated!" or "Salesperson not found." based on rows affected

**`deleteSalesperson(int empNo)`**
Deletes a salesperson by employee number.
- Use `PreparedStatement`
- Query: `DELETE FROM salesperson WHERE emp_no = ?`
- Print "Salesperson deleted!" or "Salesperson not found." based on rows affected

---

### Class 3 — `App.java`

The entry point. Create a `SalespersonManager` instance and a `Scanner`. Loop through a menu until the user picks 5, calling the right method for each choice.

---

## Things to Think About

- All database methods need `throws SQLException` or a try-catch
- Use try-with-resources to close connections — just like the lab
- `dept_no` must match a valid department (1–5) or the insert will fail due to the foreign key constraint
- The existing employees have emp_no values 10, 20, 30, 40, 50, 60 — use a different number when adding a new one
- When you call `getAllSalespeople()`, loop through the list and print each one using `toString()`

---

## Extension (if you finish early)

- Add a **View by county** option — user types "Surrey" and only Surrey salespeople are shown
- Add a **View one salesperson** option — user enters an emp_no and sees just that person, or "Not found"
- Add a **View by department** option that also shows the department name by joining with the `dept` table:
  `SELECT s.*, d.dept_name FROM salesperson s JOIN dept d ON s.dept_no = d.dept_no WHERE s.dept_no = ?`