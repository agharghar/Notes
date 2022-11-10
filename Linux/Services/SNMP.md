## SNMP Versions

There are 2 important versions of SNMP:

-   **SNMPv1**: Main one, it is still the most frequent, the **authentication is based on a string** (community string) that travels in **plain-text** (all the information travels in plain text). **Version 2 and 2c** send the **traffic in plain text** also and uses a **community string as authentication**.
    

-   **SNMPv3**: Uses a better authentication form and the information travels **encrypted** using (**dictionary attack** could be performed but would be much harder to find the correct creds that inn SNMPv1 and v2).
    

Community Strings

As mentioned before, **in order to access the information saved on the MIB you need to know the community string on versions 1 and 2/2c and the credentials on version 3.** The are **2 types of community strings**:

-   **`public`** mainly **read only** functions
    

-   **`private`** **Read/Write** in general
    

Note that **the writability of an OID depends on the community string used**, so **even** if you find that "**public**" is being used, you could be able to **write some values.** Also, there **may** exist objects which are **always "Read Only".** If you try to **write** an object a **`noSuchName`** **or** **`readOnly`** **error** is received**.**

In versions 1 and 2/2c if you to use a **bad** community string the server wont **respond**. So, if it responds, a **valid community strings was used**.


```shell
snmpbulkwalk -c [COMM_STRING] -v [VERSION] [IP] . #Don't forget the final dot
snmpbulkwalk -c public -v2c 10.10.11.136 .
snmpwalk -v [VERSION_SNMP] -c [COMM_STRING] [DIR_IP]
snmpwalk -v [VERSION_SNMP] -c [COMM_STRING] [DIR_IP] 1.3.6.1.2.1.4.34.1.3 #Get IPv6, needed dec2hex
snmpwalk -v [VERSION_SNMP] -c [COMM_STRING] [DIR_IP] NET-SNMP-EXTEND-MIB::nsExtendObjects #get extended
snmpwalk -v [VERSION_SNMP] -c [COMM_STRING] [DIR_IP] .1 #Enum all
snmp-check [DIR_IP] -p [PORT] -c [COMM_STRING]
nmap --script "snmp* and not snmp-brute" <target>
```

### SNMP RCE

https://rioasmara.com/2021/02/05/snmp-arbitary-command-execution-and-shell/

https://book.hacktricks.xyz/network-services-pentesting/pentesting-snmp/snmp-rce