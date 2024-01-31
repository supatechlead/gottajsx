---
title: "Create Virtual Environment for Python"
date: 2024-01-12T07:41:05+01:00
type: "post"
draft: false 
description: "How to Simply Use Venv Virtual Environment Manager"
showTableOfContents: true
tags: ["venv"]
---

`Venv` stands for "virtual environment," and it is a built-in module in Python for creating isolated Python environments. 

<!--more-->

Virtual environments are useful for managing dependencies and project-specific packages, allowing you to work on different projects with potentially conflicting dependencies without affecting your system-wide Python installation.

To install and use venv (Virtual Environment), follow these steps:

1. Create a virtual environment:

```bash
python3 -m venv env
```
Replace `env` with the name you want for your virtual environment.

2. Activate the virtual environment:

```bash
source env/bin/activate
```
3. Deactivate the virtual environment:

```
deactivate
```

Once the virtual environment is activated, you can install Python packages specific to your project without interfering with globally installed packages. Use the `pip` command to install packages, for example: `pip install package_name`.

Make sure you have Python 3.3 or later installed on your system. If your Python version is earlier than 3.3, you may need to use `virtualenv` to create virtual environments.
