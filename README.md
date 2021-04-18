
![power-automate-4.png](https://github.com/jambesh/tableau-power-automate/blob/main/images/power-automate-jpeg.jpg?raw=true)

# Tableau Power Automate : Automating Multisite Tableau Extract Refresh Failure.
Automate Multisites Tableau Extract Refresh Failure to Slack , Microsoft Team , Email using Postman , Tableau webhook and MS flow .
MS Flow is like If-Than-That  , with advance options for dynamic expression/formatting capability to direct output to various medium  and require no setup & coding.

* What is good about using MS Flow:
  
      *  It will be within company license for Office 365 (No additional subscription fees or trial)
      *  You can log your failure events to microsoft one drive for historical reporting.
      *  Check flow failure/Success events 
      *  Use Advance formatting , logging and communication if needed.

## Pre-Requsite : 
1) You must have Tableau Site Admin or Server Admin Role or should be able to take help from a Site admin to establish the initial login.
2) The easy automation , message customization , formatting is done through Microsoft Power Flow , So if your company using Microsoft office 365 suite then this is the solution for you.

## Software Requirement :
1) Postman (Free) API testing tool you can install for MAC/Windows  - No Account setup require . 
   This is needed for one time login to your Tableau server and to create a webhook . (  after that you can uninstall Postman ).

# Step A : Create Webhook URL & Message Customization

Part-1:
Setting up the webhook URL ,  

Part-2:
Parse the JSON response from Webhook to use in Message Customization.


Part-3:
Setting up multi site configuration and message formatting and Directing the info to Slack/MS Team.

Tableau return webhook POST success message response in JOSN and the site info is "only Site LUID"  and not the Site name, so unless you associate the Site LUID to Site name , failure from multi site to single channel or team channel is confusing .
We will handle that in a if then else mapping in MS Flow -





# Step B : Create Tableau Webhook using postman.

###### Simple Postman Installation and Import Step:
   1) Download latest Postman from https://www.postman.com/downloads/ for your sytem(MAC/Widnows)
   2) Download the two Postman Collection from github - [Tableau Webhook Setup](https://github.com/jambesh/tableau-power-automate/blob/main/Tableau%20Webhook%20Setup.postman_collection.json) and [Tableau Webhook Environment](https://github.com/jambesh/tableau-power-automate/blob/main/Tableau%20Webhooks%20Environment.postman_environment.json)
   3) Import both Tableau Webhook Setup  and Tableau Environment request collection into postman by File -->Import or the Import option within the tool.
   
###### After Import :
   1) No changes needed for tableau webhook setup file as every thing on that collection is set from Environment file to avoid manual work - Check the Test section of the collection in postman, you will notice i have placed some Java Script code to automatically parse the json result and set the environment variable for you and this is different from Tableau postman collection.
   2) Just edit the Tableau Webhook Environment you imported by clicking on Eye Icon at the top right (next to environment drop down list) and  edit to update your Tableau Environment Info/User info.
   3) What you need to update in the environment variable ?

   ![EnvironmentVariableEdit-3.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/EnvironmentVariableEdit-3.jpg?raw=true)
   
         *  For all the environment variables that need to be edit , just update the CURRENT_VALUE section and leave the initial as-is.
         *  Update  Tableau Server URL & Site Name (if the Site is Default then leave it empty)
         *  Webhook URL : This is important - Copy this URL from the Microsoft Flow that you created earlier.
         *  One of the ID/Password or Token/Secret variable  pair .
         *  if you will use the postman request Login using ID/Password - then update User Name and Password . 
         *  if you will use the postman request Login using Token Name/Secret - then update the token name and secret.
            (You can create a new Token Name/Secret from your Tableau User Profile setting

   4) Now time to login  - Select one of the Sign-In (Type) Request from left(ID/Password OR Token/Secret) and hit "Send" - if you have setup the environment variable correctly for one of login method, then this should return the 200 status with your token info/Site LUID and set all Environment variable automatically. ##  Note : This step has slight different from Tableau postman collection as this will take care of setting the Environment variable automatically rather you manually copy and paste them after login and before create the webhook.
   5) To Create the webhook Data Source Failed Event , Select Create-webhook-datasource-refresh-failed  request and hit "Send" - This should create the webhook for data source failed events. 
   6) To Create the webhook workbook failed events , select Create-webhook-workbook-refresh-failed Request and hit "Send" - This should create a webhook for Workbook failed. 
   ##### **Note : Write down the Site ID returned in the JSON  for the site you just created the webhook

## For multi Site webhook Event Setup 
   
      *  Edit and Change just the  new Site name in Tableau Environment collection and re-run step-4 through Step-6.
      *  Collect the Site LUID returned in the JSON .
      *  These SITE LUIDs , You can then update in the  Microsoft Flow Dynamic Expression to map Site LUID to Site Name.
      *  Example :  If for the first time you login to your Default site and you got the site id  d123456789
         and second time you login for a different site says "Finance" and got a  Site luid f123456789
      *  Go to Microsoft flow then in the MS flow dynamic expression replace your  Site ID &  Site Name to map and display in Slack/MS Team correctly.

  Once you update the MS Flow Dynamic expression with the right Site LUID/Site Name You shuld see real time Alert from multi Site in same channel dynamically.
    
  ## Slack Message Sample for Workbook  & Data Source Failed Events
  ![Site-1-And-Site-2-Message-Slack.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/Site-1-And-Site-2-Message-Slack.jpg?raw=true)
  ## MS Team Message Sample for Workbook & Data Source Failed Events
  ![MS-Team-event-1.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-event-1.jpg?raw=true)
  ![MS-Team-event-2.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-Event-2.jpg?raw=true)
  
   
