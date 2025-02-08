---
title: Find and replace a word from all the files in a directory
date: 2023-10-23
categories: [Linux, Tips]
tags: [linux,tips,commands]     # TAG names should always be lowercase
---

This tip make use of powerful command "sed". 
Run this command to locate and substitute a specific word within all the files in the current directory, whether they have a .txt, .cfg, .js, .html extension, or any other file type that suits your needs.
You can find sed command in most of the linux systems. But if you are on windows, follow this - [Chocolatey The Package Manager for Windows](https://www.shyju.in/posts/Chocolatey-The-Package-Manager-for-Windows/)

{% raw %}
```
sed -i -- 's/firstword/secondword/g' *.txt

```
{% endraw %}

It searches for  the text "firstword" in all the .txt files in the current direcory and replaces it with "secondword".
