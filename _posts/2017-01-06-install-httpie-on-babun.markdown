---
layout: post
title: install HTTPie on babun or cygwin
date: '2017-01-06T11:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: httpie cygwin babun windows curl REST
---

As I always forget how to do it on windows using babun or cygwin, I'll push here the steps I followed in order to reinstall the excellent [HTTPie](https://httpie.org/) command line tool for REST (and http in general) on my fresh renewed babun installation.  

First verify that _pip_ is not installed on your system by running `pip --version` in a shell.  
If the command _pip_ is not found then continue, otherwise directly go to HTTPie installation.  

- install _easy_install_:

    `wget https://bootstrap.pypa.io/ez_setup.py -O - | python`, instruction taken from [setuptools](https://pypi.python.org/pypi/setuptools#installation-instructions)

- install _pip_ using _easy_install_:

    `easy_install pip`, if command is not found try with `easy_install-2.7 pip` or `easy_install-3.4 pip` or other version depending on the python version

- install [HTTPie](https://httpie.org/):

    `pip install httpie`

You're done, enjoy calling your REST endpoints using a really powerfull CLI tool.
