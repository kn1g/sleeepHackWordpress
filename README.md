# sleeep Hack Wordpress
I noticed yesterday that my friend's Wordpress was hacked. At least a wp-plugin called **sleeep** was installed and redirects to scam sites like transandfiestas were set. But before my visitors were hijacked, I saw an error similar to: `wp-sleeeps/wp-sleeeps.php on line 26`

I could not find a lot of information about this incident, that's why I just write down what I found out so far and it might help others. 

If you have more information, please let me know! (you can open an issue or PR)

## Remove the malware

1. Bring the site down through either. I simply logged into the hoster account and changed the DB (Data Base) password as this needs to be done anyway. The site will then be unavailable because of an DB error from Wordpress. But you can also use a password protection etc.
1. Download your database dump 
1. Check the `siteurl` and `home` values in your `_option` table 
1. Search for these values if they are not your website domain. In my case it was something with `transandfiestas`. I found a couple of blog entires with js script insertions. 
1. Delete all insertions. In my case I just searched for the `<script ... src='https://transandfiestas...</script>'` part and deleted it by replacing it with nothing.
1. Change the `siteurl` and `home` values back to your website url
1. Log into yout FTP account account and delete the sleep plugin in `wp-content/plugins/` (or similar. I always confuse the fildernames)

That is all I did until now. I need to check tomorrow if I need to do more. It is just too late now. Let me know if you know about more broken stuff.
