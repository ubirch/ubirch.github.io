# Ubirch Console

The [Ubirch Console](https://console.demo.ubirch.com) provides browser based user interface for a couple of functions that allow the registration and management of things/devices, which are running the [Ubirch Nano Client](sdk).

> If you don't want to use an UI, check the [Ubirch Trust Service REST API documentation](api).

## Register and Login

Klick the **Register** link on the login page and register with the required information.
![register](img\console\002-ubirch-web-ui.png)

Login using your created account.
![login](img\console\001-ubirch-web-ui.png)

## Home
After a successful login you will see the **Home** screen, showing you some basic information like the number of your active devices.
![home](img\console\003-ubirch-web-ui.png)

## Things
The **Things** screen will list your active things and enable you, to add devices.
![things](img\console\004-ubirch-web-ui.png)

## Add Things
Click the **Add New Device** button to add a new thing to the system.
![add things](img\console\005-ubirch-web-ui.png)

You can add multiple things at once, by adding the IDs comma seperated.
![add many things](img\console\007-ubirch-web-ui.png)

If successful you will see a confirmation.
![add things ok](img\console\006-ubirch-web-ui.png)
![add many things ok](img\console\008-ubirch-web-ui.png)

And all the devices will show up in your list.
![things list](img\console\010-ubirch-web-ui.png)

And the things counter on the home screen will be updated.
![home count](img\console\013-ubirch-web-ui.png)

If adding the devices fails, you will see the following error.
![add things nok](img\console\009-ubirch-web-ui.png)

## Delete Things
Things can be deleted from the system in the things list view (or the [Things Detail View](#Things Detail View)), by clicking the delete button and again confirming with a click on the delete button in the confirmation pop up.
![del thing from list](img\console\018-ubirch-web-ui.png)

## View Things Details
By clicking a thing in the things list view you will get to the things detail view. Here you can change and save the description of a thing, delete the thing selected and view the **API configuration (apiConfig)** for this thing.
![things detail view](img\console\015-ubirch-web-ui.png)

## Logout
You can log out by clicking the logout button in the main menu and confirming your logout request.
![logout](img\console\020-ubirch-web-ui.png)

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
