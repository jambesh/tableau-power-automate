
![power-automate-4.png](https://github.com/jambesh/tableau-power-automate/blob/main/images/power-automate-jpeg.jpg?raw=true)

# Tableau Power Automate : Automating Multisite Tableau Extract Refresh Failure.
Automate Multisites Tableau Extract Refresh Failure to Slack , Microsoft Team , Email using tableau webhook and MS flow .
With MS Flow, absolutely no coding require to setup the webhook and at the same time message custimization ,additional Multisite Setup is easy.
Tableau Sent webhook POST message response in JOSN and only Site LUID and not the Site name, so unless you associate the LUID to Site name , failure from multi site to single channel or team channel is confusing as user will not know from which site extract failed. With a simple if then else for site mapping , this can be done in flow.

* What is good about using MS Flow:
*  
      *  It will be within company license for Office 365 (No additional subscription fees or trial)
      *  You can log your failure events to microsoft one drive for historical reporting.
      *  Check flow failure/Success events 

*Bonus:  Customize Failure Alert message to Team with rich text formatting . You can use ICONs etc.

## Pre-Requsite : 
1) You must have Tableau Site Admin or Server Admin Role or should be able to take help from a Site admin to establish the initial login.
2) The easy automation , message customization , formatting is done through Microsoft Power Flow , So if your company using Microsoft office 365 suite then this is the solution for you.

## Software Requirement :
1) Postman (Free) API testing tool you can install for MAC/Windows  - No Account setup require .
   This is need one time to login to your Tableau server to create a webhook and after that you can uninstall Postman.

# Step A : Create Webhook URL & Message Customization
Setting up the webhook URL , Setting up multi site configuration and message formatting using Microsoft Flow 

it is really easy to setup the webhook URL on Mirosoft Flow -  follow the instruction and you should be ready in 15 min.


# Step B : Create Tableau Webhook (Tell Tableau where to post events when it occur)

###### Simple Postman Installation and Import Step:
   1) Download latest Postman from https://www.postman.com/downloads/ for your sytem(MAC/Widnows)
   2) Download the two postman json scripts from this github repository  "Tableau Webhook Setup.* "  and "Tableau Environment.*"  into your system.
   3) Import both Tableau Webhook and Tableau Environment json file collection into Postman.
      [Check How To Import Postman Collection](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-github-repositories)
      Importing these two file/collection is pretty easy into post man - just choose import and select one file at a time .
###### After Import :
   1) No changes needed for tableau webhook setup file as every thing on that collection is set from Environment file to avoid manual work.
   2) Just edit the Tableau Webhook Environment you imported and update your Tableau Environment Info/User info.
   ![edit-env-file.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/edit-env-file.jpg?raw=true)
   3) What you need to update in the environment variable ?

   ![EnvironmentVariableEdit-3.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/EnvironmentVariableEdit-3.jpg?raw=true)
   
         *  Update  Tableau Server URL/Site Name (if the Site is Default then leave it empty)
         *  One of the ID/Password or Token/Secret variable  pair .
         *  if you will use the postman request Login using ID/Password - then update User Name and Password . 
         *  if you will use the postman request Login using Token Name/Secret - then update the token name and secret.
            (You can create a new Token Name/Secret from your Tableau User Profile setting

   4) Now time to login  - Click on one of the Login (Type) Postman Request from left(ID/Password OR Token/Secret) and hit "Send" - if you have setup the environment variable correctly for one of login method, then this should return the 200 status with your token info/Site LUID .
   5) Create the webhook magic  - select the Data Source Failed Webhook Request from left and hit "Send" - This should create the webhook for tableau data source failed events. To Create the webhook workbook failed events , select the Workbook failed Request from left pane and hit "Send" - This should create a webhook with Workbook failed. 
   * Collect the Site LUID return in the JSON  * 

   6) * For multi Site webhook Event Setup *  , Edit and Change the Site name in Tableau Environment collection and re-run the step-5 for Workbook and Data Source
      * collect the Site LUID returned in the JSON *  These SITE LUIDs You can update in the  Microsoft Flow Dynamic Expression to associate Site LUID to Site Name.

  Finally You shuld see real time Alert in all your medium of communication channel (slack/MS Team etc)
    
  ## Slack Message Sample for Workbook  & Data Source Failed Events
  ![Site-1-And-Site-2-Message-Slack.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/Site-1-And-Site-2-Message-Slack.jpg?raw=true)
  ## MS Team Message Sample for Workbook & Data Source Failed Events
  ![MS-Team-event-1.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-event-1.jpg?raw=true)
  ![MS-Team-event-2.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-Event-2.jpg?raw=true)
  
   
