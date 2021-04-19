
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

Part-1: Setting up the webhook URL 
1) Navigate to https://flow.microsoft.com  and login using your corporate id/password or Single Sign options .
Once you login, Select + Create from the left side of the Menu and then Select "Instanct Cloud Workflow" from the list of available Start from Blank at the top section.
2) This will bring up the flow builder , Give a name to your flow and under the list of available option for "Choose How To Trigger the Flow" select 
   "When an Http Request is received request" this is mostly at the bottom of the list. and click "Create"
3) Click on + Next Step 

Part-2:Parse the JSON response from Webhook to use in Message Customization.

1) In the Choose an Operation "Select Parse JSON" to bring the JSON parse  from the above step.
2) In the *Content* field text box, click and choose "BODY" from the Dynamic Content.
3) In the *Schema* field text box , Click generate from sample and copy and paste the below  Tableau standard JSON response for refresh failure events in the JOSN input section and click Done to automatically generate the right schema and structure for you .
* Sample JSON to generate the schema 
    * {
        "resource":"DATASOURCE",
        "event_type":"DatasourceCreated",
        "resource_name":"My Datasource",
        "site_luid":"8b2a95d8-52b9-40a4-8712-cd6da771bd1b",
        "resource_luid":"99",
        "created_at":"2018-11-15T17:14:45Z"
        }

4) Click + Next Step To Customize Extract Failure Time Zone , Tableau JSON return the time in UTC Universal Time ,
   *  In the Choose a Operation , Search for "Convert Time Zone" and select that.
   *  In the Time Zone Converstion Box, Click on the Base Time  text bob area and select "Created At" from the Dynamic expression (list of available field)
   *  then Click on the Source Time zone text box area and choose "(UTC) Coordinated Universal Time" 
   *  then click on the Destination Time Zone and choose the right time zone for your server or the way you want to see it, i choosed "(UTC) Pacific Time (US & Canada)" as my Time zone.
   *  Fianlly specify the date format you want to see for your extract failed time - I liked the exact time in this format  "Full date/time pattern (short time) - Monday, June 15, 2009 1:45 PM [f]"
   

Part-3: Configure Message To Slack/MS Team and customize message formatting /multi site condition.
  * +Next Step (add a new step) and select Microsoft Team from the list of available operation .
  * Then select "Post Message as Flow Bot to the Channel" and select the Team Name to which you want to direct your message and then the channel Name ("General")
  * Add a Subject Line in the subject Line Field
  * Customize Message in the  Message Field text box  by  selecting appropiate field and highlighting text in rich text font/color. 
   Notice that the converted Time field is available now under dynamic expression (this is the extract failed time converted to specific time zone and format)
   For the site name use the dynamic expression like below for multi site as place holder and later, copy the site ID from the postman login and update the correct Site ID .
   ###### if(equals(body('Parse_JSON')?['site_luid'],'6aee4c73-d241-4b5a-835a-a564e7f41999'),'Default','Site-1')

  * To add a parallel branch to send the message to slack repeat step -3 but this time instead of Selecting Microsoft Team, Select Slack and fill the info in same way. The only difference is Slack messaging don't support the same rich text editing like MS team but that is OK. 
  * in Slack configuration you can specify the ICON url for Slack bot logo for logo customization.

## Check the Video Instruction just in case it was too much to read for Part-1

[![Webhook Setup Instruction](https://github.com/jambesh/tableau-power-automate/blob/main/images/webhook-setup-thumbnail.jpg?raw=true)](https://youtu.be/Fh3JiEu2k6s)



# Step B : Create Tableau Webhook using postman.

###### Simple Postman Installation and Import Step:
   1) Download latest Postman from https://www.postman.com/downloads/ for your sytem(MAC/Widnows)
   2) Download the two Postman Collection from github - [Tableau Webhook Setup](https://github.com/jambesh/tableau-power-automate/blob/main/Tableau%20Webhook%20Setup.postman_collection.json) and [Tableau Webhook Environment](https://github.com/jambesh/tableau-power-automate/blob/main/Tableau%20Webhooks%20Environment.postman_environment.json)
   3) Import both Tableau Webhook Setup  and Tableau Environment request collection into postman by File -->Import or the Import option within the tool.
   
###### After Import :
   1) No changes needed for tableau webhook setup file as every thing on that collection is set from Environment file to avoid manual work - Check the Test section of the collection in postman, you will notice i have placed some Java Script code to automatically parse the json result and set the environment variable for you and this is different from Tableau postman collection.
   2) Just edit the Tableau Webhook Environment you imported by clicking on Eye Icon at the top right (next to environment drop down list) and  edit to update your Tableau Environment Info/User info.
   3) What you need to update in the environment variable ?

   ![EnvironmentVariableEdit-3.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/EnvironmentVariableEdit-3.jpg)
   
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
   
      *  Edit and Change the new Site name in Tableau Environment  and re-run step-4 through Step-6.
      *  Collect the Site LUID returned in the JSON .
      *  These SITE LUIDs , You can then update in the  Microsoft Flow Dynamic Expression to map Site LUID to Site Name.
      *  Example :  Say for the first time you login to your Default site and you got the site id  d123456789
         and second time you login for a different site says "Finance" and got a  Site luid f123456789 and you want to setup 
         the Alert from these two site.
      *  Go to Microsoft flow that you created earlier and edit the flow , In the Message Section of Slack/Microsoft Team 
         select dynamic expression  next to Site Name :  and replace your  Site ID &  Site Name in the expression below and paste it in the expression 
         if(equals(body('Parse_JSON')?['site_luid'],'d123456789'),'Default','Finance')
      ![Site-1-And-Site-2-Message-Slack.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/Site-1-And-Site-2-Message-Slack.jpg?raw=true)


  Once you update the MS Flow Dynamic expression for the Site Name with the right Site LUID/Site Name You shuld see real time Alert from multi Site in same channel dynamically.
    
  ## Slack Message Sample for Workbook  & Data Source Failed Events
  ![Site-1-And-Site-2-Message-Slack.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/Site-1-And-Site-2-Message-Slack.jpg?raw=true)
  ## MS Team Message Sample for Workbook & Data Source Failed Events
  ![MS-Team-event-1.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-event-1.jpg?raw=true)
  ![MS-Team-event-2.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/MS-Team-Event-2.jpg?raw=true)
  
   
