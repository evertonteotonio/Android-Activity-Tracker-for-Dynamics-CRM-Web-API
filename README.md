# Android Activity Tracker Sample App for Microsoft Dynamics CRM

These instructions describe how to build and run the sample Android Activity Tracker application. 

## Register the application

Applications that access CRM business data, using the OAuth identity provider of the organization web service, must be registered in Active Directory. For an on-premises/IFD CRM deployment, AD FS 3.0 (or later) is used to register the app. With CRM Online, Microsoft Azure Active Directory is used for app registration. The result of application registration is a client ID value and a redirect URI that must be placed into the sample application's code where indicated in the next section. These values are used in the authentication process to determine whether the app has permission to access the organization's data.

For information on how to register an application, see [Walkthrough: Register a CRM app with Active Directory](https://msdn.microsoft.com/en-us/library/dn531010.aspx).

For internal app testing, you can share a single app registration across multiple apps. An app registration contains a single client ID, but can include multiple redirect URI's. For testing, you can add a redirect URI for each app you are developing. Once you are ready to publish an app to the Play Store, the app need's its own registration.

## Build the application

Prerequisites: This sample requires Android Studio 2, JDK 1.8.0, Android 6, and CRM 2016 to build and run.

1. Load the app's project, located in the ActivityTracker folder under the project’s root folder, into Android Studio (note: if this is the first time you are opening the project, you need to import it so it properly sets up the workspace and gradle system).
2. In the project view, expand app > src > main > java > com.microsoft.activitytracker > ui.
3. Edit the SetupActivity.java file.
4. On line 46 in the code file, change the provided value of `CLIENT_ID` to the client ID value from your app registration.
5. On line 47 in the code file, change the provided value of `REDIRECT_URI` to the redirect URI value from your app registration.
6. Build the application.

## Run the application

On the mobile device where you deployed the app, choose the Activity Tracker app to start it. The following screens show how to access the features of the app.

Page | Visual Navigation | Description
------- | ---- | ----
Server and account | ![Server URL and username](./_images/1A - Server URL.png) | Enter the https:// root address of your CRM server, including the name of your domain, and the username of the logon system user account. If you do not know the URL, you can find it in the CRM web application by navigating to **Settings > Customization > Developer Resources**. An example URL would be: https://mydomain.crm.dynamics.com. Choose **Start Login** to continue.
Logon credentials |![Logon credentials](./_images/2A - Logon.png) | Enter your the system user account password, for the organization that you specified on the previous page, and choose **Sign in**.
Search |  | The main page is initially blank with no contacts showing. Choose the search icon and enter one or more letters to find all contacts whose full name contain those letters.
Contact details | ![Contact record](./_images/5A - Contact record with activity.png) | From the search results, choose a contact to display the details for that contact. On the details page, you can choose the street address to see that location on a map, the phone number to call the contact, or the email icon to send an email.
Activity Creation | ![Contact new](./_images/5A1 - Contact create types.png) | Press the + floating action button to display the different types of activities you can create. Note that only the check mark command is enabled. The other commands are just place holders.
New activity | ![New activity](./_images/6A - Adding an activity.png) | Choose the check mark icon on the details page to create a new activity for the contact. Enter the requested information. Choose the check mark again to save the activity record and return to the details page.

## Notes

Normally in oAuth you want to store the token and the refresh token so if the access token hasn't expired yet, you just reuse that one. Otherwise you pass the refresh token to get a new access token. Since you are unable to access the Token from ADAL, and you are required to pass the refresh token into `acquireTokenByRefreshToken` you have to actually store them manually as shown in this sample. If you want to continuously refresh without caring if the access token is still valid or not, all you have to store is the refresh token.