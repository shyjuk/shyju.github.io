---
title: "Odoo - Nginx - Logout URL"
date: 2018-06-14
categories: 
  - "technical"
---

If your odoo site redirects from your public address to the local IP address when logout. Add the following settings under in nginx config and set proxy\_mode = True in odoo config.

proxy\_set\_header Host $http\_host; proxy\_set\_header X-Real-IP $remote\_addr; proxy\_set\_header X-Forward-For $proxy\_add\_x\_forwarded\_for; proxy\_set\_header X-Forwarded-Proto https; proxy\_set\_header X-Forwarded-Host $http\_host; proxy\_headers\_hash\_bucket\_size 64;
