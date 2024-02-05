---
layout: post
title: Linux-TKS Is Born
category: announcement
comments: true
---

Password management is a critical task one should perform about daily. As a
former KDE user, I have all of my secrets stored in a GPG-encrypted KWallet.
I'm quite familiar with that piece of software, and I even maintained it for
several years when I got acquainted to the existing code-base. As a matter of
fact, I've added that GPG backend to KWallet.

The KDE environment is an excellent piece of work, it greatly supported my
first steps to the Linux world. But this is now an old story, and later I've
grown desktop environment agnostic. I strongly believe `less is more` and not
using a desktop environment helps one stay focused on the daily work. I won't
debate pros and cons of the desktop environments here, though.

The freedesktop.org's Secret Service API was defined in an attempt to enable
users to become desktop environment agnostics. Using it looks like  a logical
step for any desktop-agnostic Linux user. Problem is, current implementations
are still depending on a desktop environment:
* KDE's KWallet now offers this API (NOTE: that was not implemented by me)
* GOME's Keyring also offers this API

Both desktop environments are offering that API for the sake of completeness.
Problem is, when I want to install just KWallet on my system, I end-up with
whole bunch of software packages I never use even while launching KWallet.
This kind of problem is induced by the very complexity modern DE have. This is
something that cannot be undone.

Another alternative could be Keepassxc. I remember having cloned that repo,
in an attempt to start using it. However, that piece of software suffers from
the plagues GUI applications usually suffer: hopeless complexity induced by
unneeded abstractions. Contributing to free software, at least in my case,
means taking on my own free-time. I needed to be able to add Yubikey support
to it, but I eventually stopped doing it as it looked to me impossible to
grasp the internals of that piece of software in a timely manner. I remembered
the KSecrets service API implementation attempt in the process, as that also
was a hopeless wander into many layers of useless async abstractions using Qt.

Considering the above, I was growing concerned that I'll end-up being stuck
with my now quite old FSFE GPG Card deciding to fail on me, leaving me without
an elegant way to store and retrieve my passwords. Then other factors came
into play, like me having more time for free-software stuff and the Rust
language becoming the second language used by the Linux Kernel.

So here I am starting-up the next desktop agnostic password management tool.
That being said, I reserve the right to use my KDE knowledge and eventually
help that community get rid of KWallet once for all.

