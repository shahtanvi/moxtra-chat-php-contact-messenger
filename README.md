Quick Chat
======================
A rich chat application built with Moxtra's JavaScript SDK and user authentication APIs. This app shows cases the flow of embedding the Moxtra Chat module into your application, so users can communicate with their contacts inside your app.

Installation
------------
## Step 1 - Download

Download the Moxtra Chat SDK package, unzip and install on your server. The package contains the following files...

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

Before proceeding further, [https://developer.moxtra.com/nextapps](register your app) with Moxtra. You will be provided with a unique client id and client secret key.

Open config.inc.php file and update [INSERT-YOUR-APP-CLIENT-ID] and [INSERT-YOUR-APP-CLIENT-SECRET] with client id and client secret provided while registering your sandbox app in Moxtra.

## Step 3 - Create Users in Moxtra

In this step you will create few users from "user_data.csv". user_data.csv is a sample user data file provided for implementing the Quick Chat application. Each row in this CSV file represents a user and each row contains a unique identifer, the first name and last name of the user. 

To add these users to Moxtra, please run the script `http://[example.com]/contact_messenger/create_users.php` (replace `example.com` with the actual domain and path of your webserver where the Moxtra Chat SDK package has been installed). 
This script calls the Moxtra API to create the users. A successful response on the web page indicates that the users have been successfully created. 

For further information on addding users in Moxtra, please visit [https://developer.moxtra.com/docs/docs-authentication/](https://developer.moxtra.com/docs/docs-authentication/).

Note: Two users can chat with each other only if they already exist in Moxtra. We recommend setting up the users when they sucessfully sign into your application and then add them to the chat.

## Step 4 - User login

Navigate to `http://example.com/login.php` (replace `http://example.com` with the actual domain and path of your webserver). 
You will now have the option to select one of the users (created in the previous step) and "Login". You will need to implement the login and authentication for your app. In this tutorial, you are just simulating the login process.

## Step 5 - Initialize user 

Upon successful login of the user, you want to show the user's contact list. But before that, you need to show the contacts you need to initialize the logged in user.

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
