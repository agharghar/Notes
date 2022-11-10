3.3.6. Practice - Text Searching and Manipulation

```shell
awk -F "{{" '{print $2}' values_and_flags.txt | awk -F "}}" '{print $1}'  | sort -gr
```