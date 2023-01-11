# Task 6.1


## A

```
#!/bin/bash


all () {
arp -a | grep -Eo ".+\([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\)"
}

function target {
netstat -t
}

if [[ $1  == "--all" ]]
then
 all

elif [[ $1 == "--target" ]]
then
 target

else
 echo "Hello Epam! This is my script,
Input keys, exemple (./script.sh --all)
       --all: key displays the IP addresses and symbolic names of all hosts in the current subnet.
       --target: key displays a list of open system TCP ports."


fi
```

## B

### 1

```
grep -Eo '^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' $1 | sort | uniq -c | sort -n | tail -1 | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' 

```

### 2

```
cat apache_logs.txt | cut -d ' ' -f 7 | sort | uniq -c | sort -n | tail -1

```

### 3

```
grep -Eo '^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' example_log.log | sort | uniq -c | sort -nr
```

### 4
```
grep -E "\b404\b" example_log.log | cut -d ' ' -f 7
```

### 5

```
grep -Po "(?<=[0-9]{4}\:).+(?=\s.+\])" apache_logs.txt | sort | uniq -c |
sort -n | tail -1 | cut -d ' ' -f 8
```

### 6

```
grep -Po "((?<=compatible;\s).+bot/.+(?=\)))" example_log.log | uniq
```

## C

```

#!/bin/bash



myls1=`ls $1`
myls2=`ls $2`
Vardate=`date "+%T"`



if [ -d $1 ]; then

        if [ -d $2 ]; then
                if [[ $myls1 != $myls2 ]]; then


                        diff -r $1 $2 | sed "s/Only in/$Vardate/" | sed "s/$1/Create/" | sed "s/$2/Deleted/" | tee filelog



                        rsync  -rd --delete-before $1/ $2

                fi



        else
                mkdir $2 ; echo "created $2, please try again"
        fi

else
        echo "No such directory"

fi
```

Add to crontab  

> crontab -e
```

* * * * * /root/script.sh

```
