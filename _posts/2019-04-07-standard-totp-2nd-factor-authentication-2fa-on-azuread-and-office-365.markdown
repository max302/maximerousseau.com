---
layout: post
title: Standard TOTP 2nd Factor Authentication (2FA) on AzureAD and Office 365
date: '2019-04-07 02:59:37'
tags:
- microsoft
- windows
- otp
- azureus
- active-directory
- yubico
- yubikey
- ad
---

I recently migrated from Google Apps to Office 365 for my personal emails, and seeing that I wanted to more or less replicate exactly what I had with Google, on the elements on my migration checklist was setting up 2FA.   
  
Sadly, 2FA is another place where Microsoft tried to reinvent the wheel and put out an Authenticator app of their own, completely disregarding the fact that standard TOTP works perfectly fine, totally secure, and familiar to anybody who gives half a hoot about security. &nbsp;

Seeing this, I went scouring through the internet to find an alternative way of doing 2FA, hopefully through FIDO/FIDO2, but sadly this is not yet supported. I did land however on the [Yubikey support docs regarding 2FA on Office 365 and AzureAD](https://support.yubico.com/support/solutions/articles/15000016486-using-yubikeys-with-azure-mfa#Uploading_OATH_Tokens42gbwp), which mentionned the possibility of generating standard TOTP. The docs in question show multiple 2FA options checked, and a user-facing option to "configure app without notifications".

<figure class="kg-card kg-image-card kg-card-hascaption"><img src="/content/images/2019/04/Lf6QPMAB80FyvD2XFitRiFT7FvU4NJw4ew.png" class="kg-image"><figcaption>Admin-facing 2FA configuration.</figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img src="/content/images/2019/04/O0871n5RTCnqGBDM3B5fZ1rURt0CwVY5yA-1.png" class="kg-image"><figcaption>User-facing 2FA configuration procedure. </figcaption></figure>

Things aren't exactly like this anymore, probably an attempt by Microsoft to steer users to their own app.   
  
To get the standard TOP, you HAVE to uncheck all other options; the Microsoft OTP app and standard OTP simply cannot coexist. Here is how my Azure 2FA configuration is setup, available in the 365 admin center in Users -\>&nbsp;Active Users -\> More -\> Multifactor Authentication setup -\> Service Settings.

<figure class="kg-card kg-image-card"><img src="/content/images/2019/04/Screen-Shot-2019-04-06-at-10.23.32-PM.png" class="kg-image"></figure>

Special care with this configuration is to be taken if this method is used since you have no failover in case your OTP info is not available. If I'm honest, phone-based 2FA is not something I'm too keen on activating anyways, considering the amount of hack attemps that I've seen, in the bitcoin sphere specifically. On iOS, OTP auth allows you to export the OTP seed, which I have backed up physically, and configured on more than one device. That seems like a much better way of doing things.

More advanced plans of Azure AD provide more flexiblity for third-party MFA providers, but I haven't tested it yet. Passwordless is supposedly available only with Azure AD Premium subscriptions, and at 5$ a month per user, pricing seems a bit steep. In the meantime, you can still use [Yubico's excellent OTP app](https://www.yubico.com/products/services-software/download/yubico-authenticator/) and one of their keys if you want a hardware element integrated to you login routines.

