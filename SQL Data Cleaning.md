1) In the accounts table, there is a column holding the website for each company. The last three digits specify what type of web address they are using. A list of extensions (and pricing) is provided here. Pull these extensions and provide how many of each website type exist in the accounts table.

SELECT RIGHT(website, 3) AS domain, COUNT(*) number_of_websites
FROM accounts
GROUP BY 1
ORDER BY 2 DESC


2) There is much debate about how much the name (or even the first letter of a company name) matters. Use the accounts table to pull the first letter of each company name to see the distribution of company names that begin with each letter (or number).


SELECT LEFT(name, 1) AS character, COUNT(*) number_of_websites
FROM accounts
GROUP BY 1
ORDER BY 2 DESC


3) Use the accounts table and a CASE statement to create two groups: one group of company names that start with a number and a second group of those company names that start with a letter. What proportion of company names start with a letter?



SELECT SUM(numrows) rows_with_numbers, SUM(numletters) rows_with_letters
FROM
  (
    SELECT 
    	CASE  WHEN LEFT(UPPER(name), 1) IN ('0', '1', '2', '3', '4', '5', '6', '7', '8', '9')
    THEN 1 ELSE 0 END AS numrows,
    	CASE  WHEN LEFT(UPPER(name), 1) IN ('0', '1', '2', '3', '4', '5', '6', '7', '8', '9')
    THEN 0 ELSE 1 END AS numletters
    FROM
    accounts
  ) temp;



4) Consider vowels as a, e, i, o, and u. What proportion of company names start with a vowel, and what percent start with anything else?



SELECT SUM(vowel_rows) rows_with_vowels, SUM(others) others_rows
FROM
  (
    SELECT 
    	CASE  WHEN LEFT(UPPER(name), 1) IN ('A', 'E', 'I', 'O', 'U')
    THEN 1 ELSE 0 END AS vowel_rows,
    	CASE  WHEN LEFT(UPPER(name), 1) IN ('A', 'E', 'I', 'O', 'U')
    THEN 0 ELSE 1 END AS others
    FROM
    accounts
  ) temp;



5) Use the accounts table to create first and last name columns that hold the first and last names for the primary_poc.



SELECT LEFT(primary_poc, STRPOS(primary_poc, ' ') -1) first_name, RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, primary_poc
FROM accounts


6) Now see if you can do the same thing for every rep name in the sales_reps table. Again provide first and last name columns.



SELECT LEFT(name, STRPOS(name, ' ') -1) first_name, RIGHT(name, LENGTH(name) - STRPOS(name, ' ')) last_name,
name
FROM sales_reps


7) Each company in the accounts table wants to create an email address for each primary_poc. The email address should be the first name of the primary_poc . last name primary_poc @ company name .com.


WITH accounts_data AS (
  SELECT LEFT(primary_poc, STRPOS(primary_poc, ' ') -1) first_name, RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name as account_name
  FROM accounts
)

SELECT first_name || '.' || last_name || '@' || account_name || '.com'
FROM accounts_data 


8) You may have noticed that in the previous solution some of the company names include spaces, which will certainly not work in an email address. See if you can create an email address that will work by removing all of the spaces in the account name, but otherwise your solution should be just as in question 1. 



WITH accounts_data AS (
  SELECT LEFT(primary_poc, STRPOS(primary_poc, ' ') -1) first_name, RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name as account_name
  FROM accounts
)

SELECT first_name || '.' || last_name || '@' || REPLACE(account_name, ' ', '') || '.com'
FROM accounts_data 



9) We would also like to create an initial password, which they will change after their first log in. The first password will be the first letter of the primary_poc's first name (lowercase), then the last letter of their first name (lowercase), the first letter of their last name (lowercase), the last letter of their last name (lowercase), the number of letters in their first name, the number of letters in their last name, and then the name of the company they are working with, all capitalized with no spaces.



WITH accounts_data AS (
  SELECT LEFT(primary_poc, STRPOS(primary_poc, ' ') -1) first_name, RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name as account_name
  FROM accounts
),
password_tables AS(

	SELECT first_name, last_name, account_name, first_name || '.' || last_name || '@' || REPLACE(account_name, ' ', '') || '.com' AS email, LOWER(LEFT(first_name, 1)) AS ffn_character, LOWER(RIGHT(first_name, 1)) AS lfn_character, LOWER(LEFT(last_name, 1)) AS fln_character, LOWER(RIGHT(last_name, 1)) AS lln_character, LENGTH(first_name) len_fn, LENGTH(last_name) len_ln  
	FROM accounts_data
)

SELECT first_name, last_name, account_name, email, ffn_character|| lfn_character || fln_character || lln_character || len_fn || len_ln || UPPER(REPLACE(account_name, ' ', '')) AS password_field
FROM password_tables 



10) Converting the given date in a proper date format in database.


SELECT date orig_date, (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2)) new_date
FROM sf_crime_data;


11) converting the formated date from previous question into the date.


SELECT first_name, last_name, date orig_date, (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2))::DATE new_date
FROM sf_crime_data;
