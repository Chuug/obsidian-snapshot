https://tryhackme.com/r/room/ignite

I easily found the admin page with creds admin/admin, then I uploaded a reversell ziped as a image asset

The user flag was in `/home/www-data`

Then I used linPeas to get information and the root password was in the fucking database.php like I was looking for the day before

root password is `mememe` found in a file called `database.php`

fuck me