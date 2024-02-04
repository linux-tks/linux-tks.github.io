---
layout: post
title: Milestone 1 hit - The TKS DBus Service Is Alive
category: announcement
---

It looks like the end of year vacationing was a great time for hacking. The
Rust language greatly helped keeping the focus on the tasks at hand, but I still
needed several iterations over the code base to get it right and make it look
(at least to me) like a legit Rust code-base :-)

I'm proud to announce that the DBus API is working:
* the `tks-service` binary can successfully start-up as a user-level systemd
  service
* `secret-tool` can be successfully be used to interact with it
* NetworkManager stores and retrieves secrets from it

As shown on the
[ROADMAP](https://github.com/linux-tks/tks/blob/master/ROADMAP.md), the
secrets storage still goes as cleartext files on disk. So this version clearly
is not production ready.

Next-up, I'll proceed to implementing a Yubikey-backed encryption of the
secret items.

