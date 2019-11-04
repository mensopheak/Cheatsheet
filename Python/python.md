# PYTHON



##### DEBUG WITH "pdb"

```python
import pdb

pdb.set_trace()
```

> Tip: ```help <command_of_pdb>``` show the command's description
>
> Most use commands: 
>
> * ```step```: will go line by and in definition
> * ```next```: will go line by line
> * ```continue```: will go till end, or next ```set_trace()```



##### USEFUL FUNCTIONS

```python
import os
help(os)
dir(os)
```



##### INSTALL VIRTUAL ENVIRONMENT IN PYTHON3

```bash
python3 -m venv venv_name
source venv_name/bin/activate
```



##### REQUIREMENTS.TXT

List all development python packages from current development virtual environment to **requirements.txt**

```bash
source venv/bin/activate
(venv) pip freeze > requirements.txt
```

Install development python packages from requirements.txt

```bash
(venv) pip install -r requirements.txt
```



##### SHELL SCRIPT WITH PYTHON ON MAC

* In ```$HOME```, create a new folder (.scripts)

* Create a new shell script (**my_command.sh**) in **.scripts** folder

  ```sh
  # file my_command.sh
  
  #!/bin/sh
  
  # Author : Men Sopheak
  
  function hello(){
      echo "Hello world"
      pwd
      echo $HOME
      python .scripts/one.py
  }
  
  ```

  > 1. ```chmod +x my_command.sh```: add the file executable mode
  > 2. ```./my_command.sh``` : run in terminal 

* Create a new python file (**one.py**) in **.scripts** folder

  ```python
  # file one.py
  
  print("Hello from my python scrip!!!")
  ```

* In if **.zshrc**, if **.bash_profile**, if **.bashrc ** is default terminal

  ```bash
  export PATH="$PATH":"$HOME/.scripts"
  
  source ~/.scripts/my_command.sh
  ```

  > Note: 
  >
  > * Export to path variable, so that the terminal will know where to get the scripts
  >
  > * ```source ~/.scripts/my_command.sh``` load source to terminal so that we can run any function inside the shell script

* In terminal

  ```bash
  $ hello
  Hello world
  /Users/home
  /Users/home
  Hello from my python scrip!!!
  ```

  

<hr>



* ```_``` : should be treated as a non-public part of the API (whether it is a function, a method or a data member). It should be considered an implementation detail and subject to change without notice
* ```__``` : private (hide from outside world)



##### ARGUMENTS

```python
def function(positional, *arguments, **keywords):
  pass
```

> * ```*arguments``` : receives a [tuple](https://docs.python.org/3/tutorial/datastructures.html#tut-tuples) containing the positional arguments beyond the formal parameter list
>
>   ```python
>   for arg in arguments:
>   ```
>
>   
>
> * ```**keywords``` : it receives a dictionary containing all keyword arguments except for those corresponding to a formal parameter
>
>   ```python
>   for k, v in keywords.items():
>   ```
>
> **Note:**
>
> *arguments must occurs before **keywords





##### GENERATE UNIQUE RANDOM KEY

```bash
python -c 'import os; print(os.urandom(16))'
```

