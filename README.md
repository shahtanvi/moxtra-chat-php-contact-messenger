Contact Messenger
======================
A rich chat application built with Moxtra's JavaScript SDK and user authentication APIs. This app showcases the flow of embedding the Moxtra Chat module into your application, so users can communicate with their contacts inside your app.

Installation
------------
## Step 1 - Download

Download the package, unzip to install into your server. The package contains the following files...

```
	|-- config.inc.php
	|-- create_users.php
	|-- functions.inc.php
	|-- login.php
	|-- my_contacts.php
	|-- readme.txt
	|-- user_data.csv
```

## Step 2 - Configuration

Before proceeding further, ensure you have registered your app with Moxtra. Register your app at [https://developer.moxtra.com/nextapps](https://developer.moxtra.com/nextapps)

Open config.inc.php file and update the values [INSERT-YOUR-APP-CLIENT-ID] and [INSERT-YOUR-APP-CLIENT-SECRET] with the actual values of your sandbox app in Moxtra.

## Step 3 - Create Users in Moxtra

In this step you will be creating few users from "user_data.csv" (sample user database file for this application). Each record in this CSV file represent an user and each record contains an unique identifer for the user in your app, first name and last name.

To setup these test user data into Moxtra, please run the script once by visiting `http://example.com/contact_messenger/create_users.php` (replace `http://example.com` with the actual domain and path of your webserver where you installed the package). 

Executing this script will call the Moxtra API to create the user. You should see the response on successful creation of this test user data in the web page.

For API documentation on creating users in Moxtra please visit [https://developer.moxtra.com/docs/docs-authentication/](https://developer.moxtra.com/docs/docs-authentication/)

Note: Before an user is trying to chat with an other user, they should exist in Moxtra. So please setup the user in Moxtra before you add them to any chat. We recommend in your implementation to setup the user in Moxtra when they successfully sign up into your application. 

## Step 4 - User login

Navigate your browser to `http://example.com/login.php` (replace `http://example.com` with the actual domain and path of your webserver where you installed the package). 

You will have the option to select one of the users (created in the previous step) and click "Login". You will need to implement the login and authentication in your app, in this tutorial you are simulating the login process.

## Step 5 - Initialize user 

Upon successful login of the user, you want to show the user's contact list but before showing the contacts you need to initialize the logged in user. 

In this step you will first authenticate the user and get the access token:

Refer to the call ```"get_access_token($uid)"``` in ```"functions.inc.php"``` for code snippet.

Using the access token you will initialize the user in your application with the following code snippet:

```
		<script type="text/javascript">
            var options = {
                mode: "sandbox", 
                client_id: "<?php echo $CLIENT_ID; ?>", //
                access_token: "<?php echo $access_token; ?>",
                invalid_token: function(event) {
                    //Triggered when the access token is expired or invalid
                    console.log("Access Token expired, please generate a new access token!");
                }
            };
            Moxtra.init(options);
        </script>
```

## Step 5 - Show "My Contacts"

After user is successfully initilalized you will show the list of user's contacts. In this tutorial "My Contacts" will show all the users from user_data.csv file except the logged in user. 

## Step 6 - Start/Open Chat

Clicking on a contact name will execute the code to start/open chat with that particular user. The following code snippet is used to start/open the chat:

```
		<script type="text/javascript">
        function start_chat (contact_id) {
            var chat_options = {
                unique_id: contact_id,
                iframe: true,
                tagid4iframe: "chat_container",
                iframewidth: "500px",
                iframeheight: "600px",                
                autostart_note: false,
                start_chat: function(event) {
                    console.log("Chat started binder ID: " + event.binder_id);
                },
                publish_feed: function(event) {
                    console.log(event.message + " " + event.binder_id);
                },
                receive_feed: function (event) {
                    console.log(event.message + " " + event.binder_id);
                },
                error: function(event) {
                    console.log("Chat error code: " + event.error_code + " error message: " + event.error_message);
                }
            };            
            Moxtra.chatView(chat_options);
        }
        </script>
```

In this tutorial we are using the chat only view SDK (https://developer.moxtra.com/docs/docs-js-sdk/chat/#conversationview), the same logic and flow can be used with other views of Chat SDK (https://developer.moxtra.com/docs/docs-js-sdk/chat/). 

## Step 7 - See it in action

Navigate to `http://example.com/login.php` from another browser, select a different user to login and you can experience the integrated chat in action.
