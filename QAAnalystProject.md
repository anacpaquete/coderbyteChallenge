# CHALLENGE - QA Analyst Project

In this document, you will find my proposed resolution to the exercises assigned in the **QA Analyst Project**.

## 1. _What questions would you ask in order to clarify requirements?_

In order to clarify requirements I would do the following questions:

| # | Questions                                                                                                                             |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------|
| 1 | **"Create a new user by filling in the following fields: Email, First Name, Last Name and clicking the Save button"**:<ol><li>In the presented image, all fields, with the exception of the Email field, are marked as mandatory. Since the temporary password is given to the new user via email, shouldn't this be a mandatory field?</li><li>Is the 'Save' button always visible, active, and functional, or is it only displayed and enabled after all mandatory fields are correctly filled?</li><li>In case an email address already registered in the database is inserted for creating a new user, what is the expected result?</li></ol> |
| 2 | **"All fields accept alphanumeric characters, length - 6 to 30 characters"**:<ol><li>Since the e-mail address contains non-alphanumeric characters (e.g. @), doesn't this requirement invalidate the insertion of a valid e-mail address? In addition to @, an email can contain other types of special characters that are not alphanumeric.</li><li>What is the expected output when incorrect values are entered into the new user creation fields? Are alert/error messages displayed immediately after inserting invalid values or only after clicking the 'Save' button?</li><li>The length and character type of the First Name and Last Name fields shouldn't be revised? Names like "Ana Silva" and "Conceição Gonçalves" are valid and common names in the Portuguese language. However, according to this requirement, users with these names wouldn't be able to register on the website. Either because the names Ana and Silva have fewer than 6 characters, or because the names Conceição and Gonçalves are not composed solely of alphanumeric characters. Other examples of valid names which users won't be able to register because of the specified requirements: Anne-Marie, John-Paul, Vilas-Boas, D'Almeida</li></ol> |
| 3 | **"After the user is created, they receive an email with a temporary password that needs to be changed shortly"**:<ol><li>What is the expected flow in case of failure to send the email with the temporary password? Is there a procedure or fallback mechanism to handle this situation?</li><li>How are the complexity and security requirements of the temporary password sent via email handled? Are there specific guidelines for this temporary password regarding length, special characters, expiration, or other security aspects?</li><li>What requirements are needed to set up the new password?</li><li>What is the expected waiting time for receiving the email with the temporary password?</li><li>Is there a defined deadline for the expiration of the temporary password? And is there any specific guidance on how long the user should take to change the temporary password?</li><li>Does the email with the temporary password contain a link that takes the user directly to the page where they can change their password?</li></ol> |
| 4 | **"The user will use these credentials to log in to the website and have email address, first name and last name displayed on the home page"**:<ol><li>In the representative image of the website's home page, the email address is not displayed. Where on the home page is the user's email address displayed?</li></ol> |
| 5 | **"User data is saved into the database"**:<ol><li>The guideline mentions that all fields should allow alphanumeric characters with a length of 6 to 30 characters. But the specific database constraints for Email, First Name, and Last Name indicate different lengths (Email - varchar(30); First Name - varchar(10); Last Name - varchar(12)). Can you clarify if these specific limits in the database match the general guideline for character length?</li><li>Is there a mechanism to ensure uniqueness in the email field in the database to prevent duplicates or data conflicts?</li><li>Are user data stored in the database encrypted to ensure compliance with data protection regulations such as GDPR?</li></ol> |
| 6 | **"User creation should not take longer than 3 seconds"**:<ol><li>What measures or optimizations are in place to ensure that user creation process not take longer than 3 seconds?</li></ol> |
| 7 | **Other**: <ol><li>As this involves registering on a website, shouldn't there be an option to select agreement with the website's terms and conditions before completing the new user creation?</li></ol> |

## 2. _Specify 5 test cases to test this requirement_

### 1 - _Validate user creation_

**Description** - The purpose of this test case is to validate that user creation respects the following requirements:
- each field only accepts alphanumeric characters
- the minimum limit of characters in each field is 6
- the maximum number of characters in each field is 30 
- after user is created, an email with a temporary password is sent to the e-mail address provided during user creation
- received credentials are valid to login into the website;
- after user login into website: email address, first name and last name displayed on the home page.


|#|Test Steps|Test Data|Expected Result|
|-|----------------|---------------------|-----------------------------------------------------------------------------------------------------------------|
|1|Create user:<ul><li>Input email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>Press 'Save'</li></ul>|The email address must not be registered on database in tblUser.Email|User is successfuly created.|
|2|Verify that user created before receive an email related to user creation:<ul><li>Access email indicated during user creation</li><li>Visualize received e-mails</li></ul>|n/a|In the inbox there is an email related to the user creation|
|3|Verify content of received e-mail and how long it took to be sent: <ul><li>Open received email related to the user creation</li></ul>|n/a|Received email contains a temporary password that needs to be changed shortly and was sent within the expected time frame.|
|4|Verify received credentials are valid to login into website: <ul><li>Go to website</li><li>Login into website with temporary password</li></ul>|n/a|Successful login with temporary credentials and website's home page is displayed with email addres, first name and last name of the user visibles|

### 2 - _Validate duplicate user prevention_

**Description** - The purpose of this test case is to validate that is not possible to create a new user with an email address already registered on database.

**Requirements** To execute this test case it's necessary to have the following requirement(s):
- Have (at least) a valid email address registered on database in tblUser.Email

|#|Test Steps|Test Data|Expected Result|
|-|----------------|---------------------|-----------------------------------------------------------------------------------------------------------------|
|1|Create user:<ul><li>Input email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>Press 'Save'</li></ul>|The email address must be registered on database in tblUser.Email|System displays an appropriate error message indicating that the email address is already in use|

### 3 - _Validate password changes_

**Description** - The purpose of this test case is to validate that after change the temporary password:
- is not possible to login into the website with the old password
- the user can login into the website with the new password

**Requirements** To execute this test case it's necessary to have the following requirement(s):
- Have a user with a valid temporary password

|#|Test Steps|Test Data|Expected Result|
|-|----------------|---------------------|-----------------------------------------------------------------------------------------------------------------|
|1|Login into website with temporary password:<ul><li>Go to website</li><li>Insert valid user credentials and login</li></ul>|n/a|Login successfully and website's homepage is displayed. On website's homepage the option 'Change password' is available|
|2|Select option 'Change password'|n/a|User is redirected to a new page to change password|
|3|Change password|New password must be strong and respect password policy|System displays a success message indicating that password is changed|
|4|Logout website|n/a|Logout successfully|
|5|Login into website with temporary password:<ul><li>Insert old user credentials and submit the form</li></ul>|n/a|Login fails and system displays a message indicating that inserted password is incorrect|
|6|Login into website with new password:<ul><li>Insert user credentials and input new password</li><li>Submit form</li></ul>|n/a|Login successfully|

### 4 - _Validate user data registration_

**Description** - The purpose of this test case is to validate database:
- verify that the data stored in the database matches the information entered during user creation
- check if the lengths of the stored values match the specified field lengths (varchar limits)
- ensure that the data types (varchar) align with the expected data types for each field
- ensure that the system correctly reject invalid entries and doesn't save invalid data

|#|Test Steps|Test Data|Expected Result|
|-|----------------|---------------------|-----------------------------------------------------------------------------------------------------------------|
|1|Create user with invalid data:<ul><li>Input email, first name, and last name exceeding character limits or using prohibited characters</li><li>Press 'Save'</li></ul>|Invalid email: 'exceeding-character-limit@email.com', Invalid First Name: 'MoreThan10Characters', Invalid Last Name: '12-Ch@racters'|User creation successfuly fails and system displays an appropriate error message indicating why user creation failed|
|2|Create user:<ul><li>Input email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>Press 'Save'</li></ul>|The inserted email address must not be registered on database in tblUser.Email|User is successfuly created.|
|3|Access database and query the relevant tables (tblUser.Email, tblUser.FirstName, tbUser.LastName)|Queries:<ul><li>SELECT Email FROM tblUser;</li><li>SELECT FirstName FROM tblUser;</li><li>SELECT LastName FROM tblUser;</li></ul>|Data registered on table:<ul><li>**tblUser.Email** matches the email addresses entered during user creation - email entered on step 2 is between the results</li><li>**tblUser.FirstName** matches values entered in First Name field during user creation - name entered on step 2 is between the results</li><li>**tblUser.LastName** matches values entered in Last Name field during user creation name entered on step 2 is between the results</li></ul>|
|4|Check if the lengths of the stored values match the specified field lengths (varchar limits)|Query: SELECT * FROM tblUser WHERE LEN(Email) > 30 OR LEN(FirstName) > 10 OR LEN(LastName) > 12;|No results are returned|
|5|Check the database to ensure that the system correctly rejects invalid entries and doesn’t save invalid data|Query: SELECT * FROM tblUser WHERE Email='exceeding-character-limit@email.com' AND FirstName='MoreThan10Characters' AND LastName='12-Ch@racters';|No results are returned|

### 5 - _Validate user creation time_

**Description** - The purpose of this test case is to validate that user creation does not take longer than 3 seconds.

|#|Test Steps|Test Data|Expected Result|
|-|----------------|---------------------|-----------------------------------------------------------------------------------------------------------------|
|1|Create user:<ul><li>Fill in the required fields with valid values</li><li>Press 'Save'</li></ul>|Requirements to valid values:<ul><li>Email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>The email address must not be registered on database in tblUser.Email</li></ul>|User is successfuly created and user creation process completes within 3 seconds|
|2|Repeat step 1 multiple times consecutively with valid values|Requirements to valid values:<ul><li>Email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>The email address must not be registered on database in tblUser.Email</li></ul>|Each user is successfuly created and each user creation process completes within 3 seconds|
|3|Stress testing:<ul><li>Simulate concurrent user creation requests</li><li>Simultaneously initiate multiple user creation requests</li></ul>|Requirements to valid values:<ul><li>Email, first name, and last name with the minimum and maximum allowed characters (6 and 30)</li><li>The email address must not be registered on database in tblUser.Email</li></ul>|Each user is successfuly created and each user creation process does not exceed 3 seconds, even under concurrent requests|
