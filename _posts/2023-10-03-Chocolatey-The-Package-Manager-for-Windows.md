---
title: Install Chocolatey The Package Manager for Windows
date: 2023-10-23
categories: [Windows, Package Installer]
tags: [windows,chocolatey,command]     # TAG names should always be lowercase
---

[Chocolatey](https://chocolatey.org/) is a package manager for Windows operating systems.
It is a free and open-source tool that simplifies the process of installing, updating, configuring, and removing software applications and packages on Windows computers.
Follow the below steps to install Chocolatey.

## Change the execution policy of PowerShell

- Open windows PowerShell as Administrator and change the execution policy of powershell by running the below command. Accept the change by pressing 'Y'.

{% raw %}
```
  Set-ExecutionPolicy AllSigned

```
{% endraw %}

## Install Chocolatey.

{% raw %}
```
 Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
{% endraw %} 

## Install 'sed' - A powerfull and versatile text-processing tool and command-line utility

{% raw %}
```
choco install sed

```
{% endraw %}

![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/3937500f-6057-4e0d-9bce-82fc581907c3)
