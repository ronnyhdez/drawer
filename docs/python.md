# Python notes

This is the section to include all notes related to python development.


# **Set up python conda environment**

This notes are here just as a reference. **Don't use anaconda**. If I need
a tool to manage environments, it's preferable to use **pipenv** or **venv**.

I have faced many problems within projects when trying to program in python
with the virtual environments managed by anaconda. I didn't document the
problems given that I though I wasn't understanding the process, but all the
problems were gone when I uninstalled anaconda and started using pipenv.

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

# Environmental variables

If I have credentials that I need to use in one project, what I can do is to
store them as a system environmental variable and then call them later in the
python script. This can be achieve with the following steps:

 - In the terminal, create the variables:

```
export my_username=fulano
export my_password=123abc
```

 - Then, let's check if the step above was succesful:

```
echo $my_username
echo $my_password
```

They should print the values we stored.

 - Now, in the python script, let's access those values

```
print(os.environ[my_username])
print(os.environ[my_password])
```

This should return the values of the environmental variables.

There is one way to create the environmental variables in a file that we can 
include in the project and ignore it in the `.gitignore`. The steps for this
are [here](https://developer.vonage.com/blog/21/10/01/python-environment-variables-a-primer#:~:text=Environment%20variables%20are%20variables%20you,it%20connects%20to%20the%20API.)

# Working with pipenv

If I have a project already started by someone else, and it have a pip file +
`pipfile.lock` I can follow the next steps: 

 - I cloned the repo
 - There is a pip file + pipfile.lock
 - When on the directory, I run `pipenv install`
 - When everything was already installed, I ran `pipenv shell`
 - Then, I will see `(CEOS-research) (base) ronny@ronny:~/Documents/repos/github/ceos/CEOS-research$` in my terminal
 
 If I want to check where my virtualenv is: `pipenv --venv`
 
 If I installed a package with pipenv, it will be shown in the Pipfile. If I 
 need to uninstall it, I can run `pipenv uninstall name_package`. This will 
 remove the package also from the Pip files.
 
 To activate the environment, that we are working on, we need to run `pipenv shell`
 
 To check the status of the packages `pipenv check`

# Check arguments of a python function

```
dir(function_name)
```

# How to debug on python?

First, I need to install pdbpp to run in sticky mode:

```
pip install pdbpp
```
Then I need to include `breakpoint` at the beginning of the code lines that
I want to run the test.

Then, in the terminal, I can open the file with:

```
python -m pdb file.py
```
 
These are the commands:

```
sticky start para iniciar

n(ext) para siguiente linea

s(tep) para entrar a depurar una funcion

si estoy en sticky no tengo que estar poniendo el list todo el tiempo

j(ump) para brincarme por numero de linea
```












