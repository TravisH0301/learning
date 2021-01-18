# Anaconda virtual Environment
Virtual environment is an isloated environment that enables an independent project to have its own version of Python and modules. <br>
Anaconda provides an interface to create and control virtual environments. For Pythons installed without Anaconda, other modules such as `virtualenv` can be used.

## Update Conda
Prior to creating a virtual machine, conda can be updated to ensure up to date modules are installed. <br>
`$ conda --version` <br>
`$ conda update conda`

## Create Virtual Environment
A virtual environment can be installed with a specified Python version with basic Anaconda modules. <br>
This installs the virtual environment at `C:\Users\user_name\.conda\envs\`. <br>
`$ conda create -n [env_name] python=x.x anaconda`

## Activate Virtual Environment
To use the virutal environment, it has to be activated (or switched). <br>
`$ conda activate [env_name]`

## List Virtual Environment
Installed virutal environments can be listed. <br>
`$ conda info -e`

## Deactivate Virtual Environment
The virtual environment session can be deactivated. <br>
`$ source deactivate`

## Copy Virtual Environment
A copy of a virtual environment can be created. <br>
`$ conda create -n [copy_env_name] --clone [env_name_to_copy]`

## Remove Virtual Environment
Install virtual environments can be deleted. <br>
`$ conda remove --name [env-name] --all`

