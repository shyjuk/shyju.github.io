---
title: The issuing identity provider is not registered with docusign
date: 2026-09-01
categories: [Docusign, SSO, Azure]
tags: [docusign,sso,certificate]     # TAG names should always be lowercase
---

<img width="635" height="332" alt="image" src="https://github.com/user-attachments/assets/d6958844-0ec9-409b-9d39-735358a7ebda" />


This issue can sometimes occur when configuring [Azure](https://learn.microsoft.com/en-us/entra/identity/saas-apps/docusign-tutorial) as an IdP with [Docusign](https://www.docusign.com).

Azure initially create a dummy SSO certificate when the you add Docusign app from Microsoft Entra App Gallery. 
The actual certificate, “CN=Microsoft Azure Federated SSO Certificate,” is create only after the page is refreshed.

If you are using Azure make sure the SSO certificate Common Name is "Microsoft Azure Federated SSO Certificate".


<img width="893" height="612" alt="image" src="https://github.com/user-attachments/assets/867feb91-3223-4881-85df-4383fa17a835" />


1. Download the latest Base64 Certificate from your Identity Provider.
2. Log into Docusign Admin -> Identity Providers.
3. Edit your active IdP in Docusign and upload the new certificate.
