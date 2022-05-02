# Set up a timer

I created a file call timer.sh

Inside it contains this:

```
#!/bin/bash

date1=`date +%s`; while true; do 
   echo -ne "$(date -u --date @$((`date +%s` - $date1)) +%H:%M:%S)\r";
```
