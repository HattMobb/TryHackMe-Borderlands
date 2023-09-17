# TryHackMe-Borderlands
A walkthrough/ write-up of the "Borderlands" box on TryHackMe (work in progress)


The Network for this test was laid out as follows:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/631a908b-23e2-4bbd-a273-425834835498)


Scan reveals a few common open ports:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/98878bab-7977-4174-a813-6d86854271f3)


The hosted web page was a simple sign in page:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/9770ee51-8b57-4e5b-9f61-92dd3f647051)


Further enumeration revealed a git repository:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/398a631d-cec7-4a64-b8ed-dd2a35aaaa28)


Heading to /.git/config reveals a user :

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/cdb4c36e-6fc5-416b-8efd-80f4103de7db)

Cant access the main repo but after doing some searching I found GitHack (https://github.com/lijiejie/GitHack):

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/cdb50cbf-73c2-482b-90ec-78a6b62aecbe)

After searching through the repo, I found I was able to retreive results by tampering with the URL:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/7d9abdba-f341-4646-8844-0a1712c8bcd1)

Double checking the `home.php` file, it seemed the `documentid` parameter was vulnerable to SQLi so I used SQLmap to check:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/2e0717a1-055d-4bcc-b7ea-06289bda86a0)


![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/b45cf9d1-1d05-4c34-9bec-f9bdf1a67aa7)

Suspicions confirmed!

I remembered it was possible to gain a shell using sqlmap but I first needed the databases used by the site:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/0309c2f6-ca9c-421e-82a9-90df1331efa9)

Once I had a target I used this to upload a backdoor via sqlmap:

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/010f106e-4640-4536-b6dc-054b92feb8d1)

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/a682a678-4211-4386-b8d4-8c5c52773394)

This allowed me to upload my shell script for a reverse shell (created via msfvenom, caught with metasploit handler):

![image](https://github.com/HattMobb/TryHackMe-Borderlands/assets/134090089/8fe86930-f43b-4c43-9dd0-4afc6551b984)



