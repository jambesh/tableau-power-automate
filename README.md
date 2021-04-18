
![power-automate-4.png](https://github.com/jambesh/tableau-power-automate/blob/main/images/power-automate-jpeg.jpg?raw=true)

# Tableau Power Automate : Automating Multisite Tableau Extract Refresh Failure.
Automate Multisites Tableau Extract Refresh Failure to Slack , Microsoft Team , Email using tableau webhook and MS flow .
With MS Flow, absolutely no coding require to setup the webhook and at the same time message custimization ,additional Multisite Setup is easy.
Tableau Sent webhook POST message response in JOSN and only Site LUID and not the Site name, so unless you associate the LUID to Site name , failure from multi site to single channel or team channel is confusing as user will not know from which site extract failed. With a simple if then else for site mapping , this can be done in flow.

* The good thing about the flow is 
      it will be within company license .
      No additional external subscription or site to be use for security.
      you can log your failure events to microsoft one drive for historical reporting.
      Check flow failure/Success events 

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


# Step B : Direct Tableau to Post Data Refresh Events to your webhook

###### Simple Postman Installation and Import Step:
   1) Download latest Postman from https://www.postman.com/downloads/ for your sytem(MAC/Widnows)
   2) Download the two postman json scripts from this github repository  "Tableau Webhook Setup.* "  and "Tableau Environment.*"  into your system.
   3) Import both Tableau Webhook and Tableau Environment json file collection into Postman.
      [Check How To Import Postman Collection](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-github-repositories)
      Importing these two file/collection is pretty easy into post man - just choose import and select one file at a time .
###### After Import :
   4) No changes needed for tableau webhook setup file as every thing on that collection is set from Environment file to avoid manual work.
   5) Just edit the Tableau Webhook Environment file and update your Tableau Environment Info/User info to create your webhook
   ![edit-env-file.jpg](https://github.com/jambesh/tableau-power-automate/blob/main/images/edit-env-file.jpg?raw=true)


