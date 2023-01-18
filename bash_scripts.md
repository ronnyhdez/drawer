# Bash Terminal topics

Probably a bit messy given that I use this space to export notes when solving
things.

## **Set up a timer**

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

## **Set up python conda environment**

 - Check if there are already envs:
 
```
conda info --envs

# conda environments:
#
base                  *  /home/ronny/anaconda3
gee                      /home/ronny/anaconda3/envs/gee
gpp                      /home/ronny/anaconda3/envs/gpp
satellite                /home/ronny/anaconda3/envs/satellite
```

 - To activate/deactivate:
 
```
conda activate
conda deactivate
```

 - There is a difference between:
 
```
apt install spyder
conda install spyder
```

 - To create a new env:
 
```
conda create --name gee python = 3.5
```