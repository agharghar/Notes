![[Pasted image 20220805061925.png]]
![[Pasted image 20220805062331.png]]

```bash
if [ <some test> ]
then
<perform action>
elif [ <some test> ]
then
<perform different action>
else
<perform yet another different action>
fi
```

```bash
if [ $USER == 'kali' ] && [ $HOSTNAME == 'kali' ] # classic AND
then
echo "Multiple statements are true!"
else
echo "Not much to see here..."
fi
```

```bash 
if [ $USER == 'kali' ] || [ $HOSTNAME == 'pwn' ] # Classic OR
then
echo "One condition is true, this line is printed"
else
echo "You are out of luck!"
fi
```

(&&) boolean operator  executes a command only if the previous command succeeds (or returns True or 0): 

![[Pasted image 20220805062822.png]]

(||) operator is the opposite of AND (&&); it executes the next command only if the previous command failed (returned False or non-zero):

![[Pasted image 20220805062919.png]]


```bash
for var-name in <list>
do
<action to perform>
done
```

```bash
for ip in $(seq 1 1 10); do echo 10.11.1.$ip; done
for i in {1..10..1}; do echo 10.11.1.$i;done
```

```bash
while [ <some test> ]
do
<perform an action>
done
####
while [ $counter -lt 10 ]
do
echo "10.11.1.$counter"
((counter++))
done
```

```bash
#!/bin/bash
# passing arguments to functions
pass_arg() {
echo "Today's random number is: $1"
}
pass_arg $RANDOM
```