# Developer Exam

We're really excited that you've made it this far in the process!

The resources you'll find in this repo will help you answer the following questions:

## JQUERY

1) **The Admin edit form**
We use CanCan (https://github.com/ryanb/cancan) (in addition to other things) to handle our user authorization.  The form in ./jquery/admin-edit.html contains a simple form inspired by the real thing.


*  Using the markup ./jquery/admin-edit.html, add a script that does the following:
    *  Validate the form.  Any textbox that has an accompanying label with a required '*' must contain data.
        *  Do **NOT** use a third party validation library.
    *  Performs a 'water falling' enabling and disabling of roles in the following way:
        *  If no roles are selected, all checkboxes are enabled and clear of any checkmarks
            *  You should always be able to return to the empty state where nothing is selected.
        *  If 'Employee' is selected, all checkboxes are enabled
        *  If 'Manager' is selected, 'Employee' is diabled & unchecked
        *  If 'Standard Admin' is selected, 'Manager' & 'Employee' are disabled & unchecked
        *  If 'Senior Admin' is selected, 'Standard Admin', 'Manager', & 'Employee' are disabled & unchecked

- - -

2) **Selectable & Draggable**
jQuery UI is an easy way to add functionality to a page that's already using jQuery.  It has useful plugins like... 

*  Droppable (http://jqueryui.com/demos/droppable/)
*  Draggable (http://jqueryui.com/demos/draggable/)
*  Selectable (http://jqueryui.com/demos/selectable/)

...among others.  

Draggable & droppable work well together and are really useful.  Unfortunately, selectable doesn't work with draggable (which makes sense when you think about it).
This is where you'll come in:

* Using the markup in ./jquery/selectdrag.html (which is pulling in jquery & the full jqueryui from the google cdn), add a script that does the following:

   *  Clicking on a row should deselect every other row except the on that was clicked on.
   *  Shift + click should select all of the rows between the currently selected row and what was most recently clicked on
        *  If there isn't a previously selected row, the 'shift+click' should simple select the recently clicked row.
   *  Once rows have been selected, the user should be able to drag rows into the 'the void'.
        *  Fire an 'alert()' that says "You have dragged ### rows into the void.", where '###' is the number of rows dragged into the void.

## SQL
We're a MySQL shop so this test is MySQL centric.  The './sql/setup.sql' file contains 'creates' and 'inserts' that only work for MySQL databases.  
Also, though questions (1) & (2) are standard SQL questions, question (3) must be answered for MySQL databases.

** _If you're a Postgres user, you'll have to perform your own translations to get the schema into your local db._ **

Based on the schema & data ...

1) Find the users who are either a 'writer' or an 'editor' as efficiently as possible (You can only use the keyword 'SELECT' once):

REQUIRED OUTPUT COLUMNS:
_The output columns MUST use labels that match the requested data below_

*  user id
*  first name
*  last name
*  role name
*  state

2) Find the count of all users WITHOUT addresses that are 'writers' and have '9' in their 'first_name' as efficiently as possible (You can only use the keyword 'SELECT' once):

REQUIRED OUTPUT COLUMNS:
_The output column MUST use a label that matches the requested data below_

*  'homeless writers'

3) Add indexes that would make the query in question 2 run faster.


## RAILS
The models in this folder correspond to the database we created in the SQL section.  Please perform the following tasks and add your work to the ./rails folder:

1) Create **2** Named Scopes that, when **chained**, can return all 'admins' that live in a specific state.
Here are some example result sets the scopes should be able to get:

*  Scope1: All of the 'writers'
*  Scope2: Everyone that lives in 'FL'
*  Scope1 + Scope2: All of the 'editors' that live in 'MA'

2) Add validation on the Address model.  

*  We should only create an Address record if the zip code contains exactly 5 numbers.

3) Write 2 **UNIT** tests to confirm that your validation in Question(2) works.  Those test should...

*  Confirm that **VALID** records **PASS** validation and **CAN** be created.
*  Confirm that **INVALID** records **FAIL** validation **CANNOT** be created.

4) The relationship between Users and Roles is pretty limited.  

*  Update the relationship in 'user.rb' & 'role.rb' to allow a single user to have multiple roles.  
*  Create any new files you'll need.

5) Write a data migration script (in Ruby or SQL) that updates the existing records in the database to use the new code you created in Question (4).


## RUBY 
Create a ruby daemon (using the deamon gem & assuming Ruby 1.8.7 interpreter) that monitors a domain.  Let's use the example domain of 'http://www.thisdomaindoesnotexist.com'.  Your solution should... 

*  Make a GET request to **http://www.thisdomaindoesnotexist.com/** every 5 seconds.  
*  If the **thisdomaindoesnotexist** server responds with a 400 or 500 response codes **OR** cannot be found, the daemon should write to a log file.  The line writen should contain 
    *  A Date & Time stamp
    *  The HTTP code if available
    *  A single sentence explaining the error. 
    *  EX: [Fri Oct 07 16:24:34 -0400 2011][200] The page is fine.
*  Here are some additional details on how the daemon should behave:
    *  The daemon should ONLY write to the log when the current repsonse differs from the last response recieved
    *  For example if the following things happened to **thisdomaindoesnotexist.com** in the following order over 20 minutes
        1.  [00:00] 200; The page is functioning
        2.  [05:00] The server gets restarted and is unreachable
        3.  [05:30] 200; The server comes back up and the page is functioning
        4.  [15:00] 500; There's an error when serving the page
        5.  [17:00] 200; The error on the page is fixed
        6.  [18:00] 404; The page is cannot be found

        ...  The log should have 6 lines, but the daemon should be running for all 20 minutes.

Add your solution to ./ruby