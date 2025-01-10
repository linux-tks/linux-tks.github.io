---
layout: post
title: We can now import KWallet passwords into the AES/GCM encrypted backend
category: announcement
comments: true
---

Quite a catchy title, isn't it? But wait, there is more. Here it is all, at a glance:

* Secrets are being stored into an AES/GCM encrypted storage
* The `tks-service` can accept secrets from the `Network Manager` or any other Secret Service-compatible client
* The accompanying tool `tks-cli` is now able to import KWallet XML exported files

---

Winter vacationing is nice, but well, it's winter and I like Summer time. So, plenty of indoors time for me these days.
While not playing the guitar or hanging around with friends, I managed to finish the most important piece of work: we
now have a working data storage backend, using OpenSSL AES-GCM encryption.

To activate this new AES/GCM backend, please define this configuration file:

~/.config/io.linux-tks/service.toml
```config
# configuration options for tks-service
# default values are shown below, uncomment to override

[storage]
kind = "tks_gcm"
#path = "$HOME/.local/share/io.linux-tks/storage"
```

The `path` option shows the default value it gets. That's the location where the data will be managed.

Once above file created, please start the `tks-service`. As this project is yet to be released, we need to use it directly out of the Rust sources. Please install Rust, then clone the [tks](https://github.com/linux-tks/tks) repository prior to proceeding with the next command:

```bash
$ source ~/.cargo/env # this initializes the Rust toolchain you've previously installed
$ RUST_LOG=trace cargo run
```

Above command might fail at compile time, if you need more stuff installed. For instance, you may need to install
`pinentry` tool, which offers a secure way to prompt for confirmations and for passwords. Carefully inspect the error
messages and install what's missing. If you need help, just drop me a line.

This will launch the TKS Secret Service implementation in tracing mode, showing plenty of information about its
internals. At the very top, you'll notice information coming from `tks_servei::storage` explaining how the storage is
being initialized.

Working condition can be confirmed in several ways. First and the most important one is that the program did not finish.
Indeed, it is intended to work as a service and should never return.

Now you may install, say, `d-feet`, launch it and look for `org.freedesktop.secrets` in the `Session Bus` section. While
interacting with `d-feet`, the output in the `tks-service` console launched above should show interactions progress.

The storage may have been initialized, but the encryption password is yet to be defined. Yes, for the moment, the GCM
backend uses passwords. In the future, I plan to connect it with the TPM and you'll get it automatically decrypted upon
Linux session start.

The storage password will be defined upon the very first access to the storage.

OK, so let's import our KWallet passwords into it, and in the process, have the GCM storage fully initialized. We'll
need the `tks-cli` tool for this:

```bash
tks/tks-cli $ cargo run -- --help
Import operations

Usage: tks-cli import [OPTIONS] <COMMAND>

Commands:
  kwallet  This command imports an XML file obtained by using KWalletManager's "export as XML" feature
  gnome    Import from GNOME Keyring
  pass     Import from PASS
  help     Print this message or the help of the given subcommand(s)

Options:
  -v, --verbose...  Increase logging verbosity
  -q, --quiet...    Decrease logging verbosity
  -h, --help        Print help
```

Please ignore the eventual compilation warnings displayed prior to the output shown above. And BTW, that exact output
also subject to change.

```bash
tks/tks-cli $ cargo run -- import kwallet --help
```

Above command will output the full KWallet import documentation. Just follow the steps described in there to get your KWallet data
imported into your new Secret Service. Upon first invocation of the tool, you'll notice a couple of prompts coming out
of the `tks-service` via `pinentry`. The first prompt will ask for permission of use - this is still under development -
and the second one will ask you to define a password. This password will be used from now on upon unlocking the Secret
Service. Remember, in the future, this will no longer be needed, but more work is to be done around the TPM and the PAM
module for that to happen.

From this point on, it will be possible for you to use any Secret Service-compatible tool to query for the data. For
instance, I used this kind of prompt to read a password I've imported from KWallet:

```bash
$ secret-tool lookup "label" "<some label you have imported>"
```

Before leaving, just let the `tks-service` run in the background. You'll notice it will already be used by Secret
Service-compatible clients, such as the Network Manager. It uses the Secret Service to store the WiFi passwords.

