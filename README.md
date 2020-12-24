# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of twelve (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. It is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features in the future, hoping to create a multi-faceted system for a Student Portal Web Application.

<br>

## Entity Relationship Diagram (ERD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database is an insource that I created using the following tools:

<a href="https://lucid.app/" target="_blank">Lucid Chart Diagram tools</a> &nbsp;&nbsp; <a href="http://online.visual-paradigm.com/" target="_blank">Online Visual-Paradigm tools</a> &nbsp;&nbsp; <a href="https://www.mockaroo.com/" target="_blank">Mackaroo Mockup data</a> &nbsp;&nbsp; <a href="https://www.convertcsv.com/csv-to-sql.htm" target="_blank">sql converter</a> &nbsp;&nbsp; <a href="https://fakenumber.net/phone-number/philippines" target="_blank">fake number generator</a> &nbsp;&nbsp; <a href="https://www.dcode.fr/values-separation" target="_blank">Varchar decoder</a> &nbsp;&nbsp; <a href="https://www.random.org/strings/?mode=advanced" target="_blank">Random string generator</a>

### Entity Names and Description

**`users`** - is responsible for storing necessary data upon the creation of a user and when its modified.

**`users_detail`** - is responsible for storing necessary data upon the creation and modification of a user that has identified itself as a student.

**`user_class`** - is responsible for storing necessary data of a classifier for users which is used to assign each user a user level to identify to when interacting with the system.

**`schools`** - is responsible for storing necessary data upon the creation of a school and when its modified.

**`articles`** - is responsible for storing necessary data upon the creation of an article and when its modified.

**`subjects`** - is responsible for storing necessary data upon the creation of a subject and when its modified.

**`author_subscriptions`** - is responsible for storing necessary data upon user subscription of the authors and when its modified.

**`articles_comment`** - is responsible for storing necessary data upon the creation and modification of user comment within an article (creates social engagement among users).

**`articles_reply`** - is responsible for storing necessary data upon the creation and modification of user replies within a comment.

**`cities`** - is responsible for storing necessary data of a city.

**`states`** - is responsible for storing necessary data of a state.

**`countries`** - is responsible for storing necessary data of a country.

<br />

## Functional Dependency Diagram (FDD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/FDD.svg)

This diagram shows that each entities had undergone normalization removing all dependencies that can cause anomalies.

<br />

## Complex Queries associated with the database

Here are a list of queries with their sample output from the DBRMS: 
<br>

><p>Note:<br>Click if you notice a dropdown button labeled <b>Show more...</b> <br> It will show the corresponding result of the query.
</p>


* ***Stored Procedures***
   1. **`Query 1: `**
      ```SQL
      DELIMITER //
      CREATE PROCEDURE insertUser(
      -- users table
      IN ucl int(11),
      IN un varchar(50),
      IN pw varchar(50),
      IN rec varchar(20),
      IN em VARCHAR(80),
      -- users_detail table
      IN fn varchar(30),
      IN ln varchar(30),
      IN ct varchar(16),
      IN sadd varchar(80),
      IN city_id int(11),
      IN schid int(11)    
      )

      BEGIN

      -- assign the next increment value to a variable
      SELECT `AUTO_INCREMENT` INTO @ai
         FROM  INFORMATION_SCHEMA.TABLES
            WHERE TABLE_SCHEMA = 'studentportal'
               AND TABLE_NAME = 'users_detail';
      
      INSERT INTO users_detail 
         ( fname, lname, contact_no, saddress, city_id, school_id ) 
         VALUES
            ( fn, ln, ct, sadd, city_id, schid );

      INSERT INTO users
         ( user_id, u_cl_id, uname, pword, rec_code, email ) 
            VALUES
               (@ai, ucl, un, pw, rec, em);

      END //
      DELIMITER ;
      ```
       <details open>
       <summary>Show more...</summary>

       **`Query for the calling program:`**
       ```SQL
       -- check the total rows before calling the procedure to get the initial number
         SELECT COUNT(user_id) FROM users_detail;
         SELECT COUNT(user_id) FROM users;
       ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-1.PNG)

       ```SQL
         -- call the procedure using dummy data as parameters
         CALL insertUser(
            10001,
            'username123',
            'password123',
            'MyCoD3',
            'email@email.com',
            'John',
            'Doe',
            '63-909-555-4117',
            'km 11 Bayview, Sasa',
            5,
            3005
         );

         -- check the total rows again after the procedure is called which should now have 1 row added to it
         SELECT COUNT(user_id) FROM users_detail;
         SELECT COUNT(user_id) FROM users;
       ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-2.PNG)
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-3.PNG)
        </details>
       
       <br>

   2. **`Query 2: `**
       ```SQL
         DELIMITER //

         CREATE PROCEDURE selectUser(
         IN uid INT(11),
         OUT un VARCHAR (30),
         OUT em VARCHAR(80),
         OUT sadd VARCHAR(80),
         OUT stat BOOLEAN
         )
         -- SELECT but not show the values that is received from this statement and assign it to different variables
         SELECT users.uname, users.email, users_detail.saddress, users_detail.is_active INTO un, em, sadd, stat FROM users INNER JOIN users_detail ON users.user_id = 		users_detail.user_id WHERE users.user_id = uid //

         DELIMITER ;
       ```
       <details>
       <summary>Show more...</summary>

        **`Query for the calling program:`**
        ```SQL
         -- call procedure
         CALL selectUser(
            10000418,
            @un,
            @em,
            @sadd,
            @stat
         );

         -- SELECT the variables one more time. This time, we are selecting it with the intention of showing the returned value
         SELECT @un, @em, @sadd, @stat;
        ```
        `Result:`
        ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-1.PNG)
        </details>

        <br>

    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```

* ***Triggers*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```

* ***Functions*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```
       
* ***Transactions*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```