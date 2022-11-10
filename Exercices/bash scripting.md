```bash
#!/bin/bash

echo "Please enter a username:" ; read username
echo "Now enter a group:" ; read group

if  id -u $username &>/dev/null  &&  getent group $group &>/dev/null 
then
        if  groups $username | grep $group &>/dev/null 
        then
                echo "Membership valid!"
        else
                echo "Membership invalid but available to join."
        fi
elif  id -u $username &>/dev/null 
then
        echo "One exists, one does not. You figure out which."
elif  getent group $group &>/dev/null 
then
        echo "One exists, one does not. You figure out which."
else
        echo "Both are not found - why are you even asking me this?"
fi
```
