# oracle-sql-testsSQL> CREATE TABLE Users (
  2      user_id INT PRIMARY KEY,
  3      username VARCHAR(50),
  4      email VARCHAR(100)
  5  );

Table created.

SQL> CREATE TABLE Roles (
  2      role_id INT PRIMARY KEY,
  3      role_name VARCHAR(50)
  4  );

Table created.

SQL> CREATE TABLE Permissions (
  2      permission_id INT PRIMARY KEY,
  3      permission_name VARCHAR(50)
  4  );

Table created.

SQL> CREATE TABLE UserRoles (
  2      user_id INT,
  3      role_id INT,
  4      PRIMARY KEY (user_id, role_id),
  5      FOREIGN KEY (user_id) REFERENCES Users(user_id),
  6      FOREIGN KEY (role_id) REFERENCES Roles(role_id)
  7  );

Table created.

SQL> CREATE TABLE RolePermissions (
  2      role_id INT,
  3      permission_id INT,
  4      PRIMARY KEY (role_id, permission_id),
  5      FOREIGN KEY (role_id) REFERENCES Roles(role_id),
  6      FOREIGN KEY (permission_id) REFERENCES Permissions(permission_id)
  7  );

Table created.

SQL> INSERT INTO Users (user_id, username, email) VALUES (1, 'Alice', 'alice@example.com');

1 row created.

SQL> INSERT INTO Users (user_id, username, email) VALUES (2, 'Smith', 'smith@example.com');

1 row created.

SQL> INSERT INTO Roles (role_id, role_name) VALUES (1, 'Admin');

1 row created.

SQL> INSERT INTO Roles (role_id, role_name) VALUES (2, 'User');

1 row created.

SQL> INSERT INTO Permissions (permission_id, permission_name) VALUES (1, 'Read');

1 row created.

SQL> INSERT INTO Permissions (permission_id, permission_name) VALUES (2, 'Write');

1 row created.

SQL> INSERT INTO UserRoles (user_id, role_id) VALUES (1, 1);

1 row created.

SQL> INSERT INTO UserRoles (user_id, role_id) VALUES (2, 2);

1 row created.

SQL> INSERT INTO RolePermissions (role_id, permission_id) VALUES (1, 1);

1 row created.

SQL> INSERT INTO RolePermissions (role_id, permission_id) VALUES (1, 2);

1 row created.

SQL> INSERT INTO RolePermissions (role_id, permission_id) VALUES (2, 1);

1 row created.

SQL> UPDATE Users
  2  SET email = 'alice_new@example.com'
  3  WHERE user_id = 1;

1 row updated.

SQL> UPDATE Users
  2  SET email = 'smith_new@example.com'
  3  WHERE user_id = 3;

0 rows updated.

SQL> UPDATE Users
  2  SET email = 'smith_new@example.com'
  3  WHERE user_id = 2;

1 row updated.
SQL> DELETE FROM UserRoles
  2  WHERE user_id = 1;

1 row deleted.

SQL> DELETE FROM Users
  2  WHERE user_id = 1;

1 row deleted.

SQL> INSERT INTO Users (user_id, username, email) VALUES (1, 'Annick', 'annick@example.com');

1 row created.
SQL> INSERT INTO Users (user_id, username, email) VALUES (3, 'Samantha', 'samantha@example.com');

1 row created.

SQL> SELECT Users.username, Roles.role_name
  2  FROM Users
  3  JOIN UserRoles ON Users.user_id = UserRoles.user_id
  4  JOIN Roles ON UserRoles.role_id = Roles.role_id;

USERNAME
--------------------------------------------------
ROLE_NAME
--------------------------------------------------
Smith
User


SQL> SELECT Roles.role_name, Permissions.permission_name
  2  FROM Roles
  3  JOIN RolePermissions ON Roles.role_id = RolePermissions.role_id
  4  JOIN Permissions ON RolePermissions.permission_id = Permissions.permission_id;

ROLE_NAME
--------------------------------------------------
PERMISSION_NAME
--------------------------------------------------
Admin
Read

Admin
Write

User
Read
SQL> UPDATE Users SET email = 'newalice@example.com' WHERE user_id = 1;

1 row updated.
SQL> CREATE USER C##smith IDENTIFIED BY password;

User created.
SQL> GRANT SELECT ON Users TO C##smith;

Grant succeeded.
SQL> SELECT username
  2  FROM Users
  3  WHERE user_id IN (
  4      SELECT user_id
  5      FROM UserRoles
  6      WHERE role_id = (SELECT role_id FROM Roles WHERE role_name = 'Admin')
  7  );

no rows selected

SQL> SELECT role_id
  2  FROM Roles
  3  WHERE role_name = 'Admin';

   ROLE_ID
----------
         1
