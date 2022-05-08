# Bash things

## Set up a timer

I created a file call timer.sh

Inside it contains this:

```
#!/bin/bash

date1=`date +%s`; while true; do 
   echo -ne "$(date -u --date @$((`date +%s` - $date1)) +%H:%M:%S)\r";
```
Then, to run the script without thinking too much, I created an alias:

```
alias timer="bash /home/ronny/timer.sh" 
```

So, any time I open the terminal, I can have a timer just typing `timer`

# Encrypt file

zip --encrypt dato_sobre.zip dato.txt

# Create a hash value

```
echo 'Hello World' | shasum -a 256
```
