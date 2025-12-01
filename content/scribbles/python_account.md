---
title: Python Installation 
draft: false
tags: ['python','dev']
date: 2025-11-30
---

reference - [Python Programmer](https://www.youtube.com/watch?v=28eLP22SMTA&ab_channel=PythonProgrammer)

>[!note] The following note is for windows systems

### Download the Python version

Download the [official](https://www.python.org/downloads/windows/) python version for Windows
Choose ==Custom installation== within python installer and choose a custom path dedicated to that version  
For example: D:\python\3.13.3

>[!info] Do not add installation to path


### Creating Python Project

Create a python project directory  
For example: D:\projects\python\project1  

Create a python virtual environment for the project  
```bash
cd D:\projects\python\project1

D:\python\3.13.3\python -m venv environment_name
```
This will create a python virtual environment under project1 that depends on python 3.13.3 version only. 

>[!info] Virtual environments in Python are like isolated workspaces where you can install dependencies without affecting the system-wide Python installation or other projects.

### Activiate The Virual Environment

Validate if the virutal environment is created or not by using the tree command

```bash
cd D:\projects\python\project1

tree
```
If environment is created, activate the environment using the activate script

```bash
cd D:\projects\python\project1

environment_name\Scripts\activate
```
Once activated, check if python is active within the environment

```bash
python --version
# should output the version used to create the environment
```
