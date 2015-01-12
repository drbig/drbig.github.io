---
layout: post
title: "SSH crypto config distilled"
description: "The defaults are bad for you."
category: computers
tags: [security, sysadmin]
---
{% include JB/setup %}

Most people are aware that the defaults for OpenSSH are relatively poor security-wise, both for the client and server elements. I've been in this category for relatively long time, though didn't do anything about it under the usual excuse of 'who would even try to seriously hack my boxes'. Now I still believe this, but after doing the Matasano [crypto challenges](https://github.com/drbig/matasano) I can personally attest that breaking crypto can be ridiculously easy in practice. That and the excellent post [here](https://stribika.github.io/2015/01/04/secure-secure-shell.html). What I'm doing here is distilling that post for both easy copy & paste (but please read the caveats below first) and historic reference.

#### Caveats

The thing about crypto is that you *really* need to understand what you're doing to minimise the probability of doing something stupid that will break the whole thing. Having said that read the original post [here](https://stribika.github.io/2015/01/04/secure-secure-shell.html). Also bear in mind that there are servers/services out there that won't work with better crypto (due to misconfiguration, on purpose or otherwise). There are also some people with opinion that OpenSSH is considered [harmful](http://harmful.cat-v.org/software/ssh).

Besides editing config files you may need to do some additional work (e.g. regenerate key files). Also note that disabling password-based auth without having keys in place may not be such a good idea.

#### Server config

    # key exchange
    KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
    
    # auth
    Protocol 2
    HostKey /etc/ssh/ssh_host_ed25519_key
    HostKey /etc/ssh/ssh_host_rsa_key
    PasswordAuthentication no
    PubkeyAuthentication yes
    
    # ciphers
    Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
    
    # integrity
    MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-ripemd160-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,hmac-ripemd160,umac-128@openssh.com

Things you'll probably need to run:

    cd /etc/ssh
    rm ssh_host_*key*
    ssh-keygen -t ed25519 -f ssh_host_ed25519_key < /dev/null
    ssh-keygen -t rsa -b 4096 -f ssh_host_rsa_key < /dev/null
    ssh-keygen -G "${HOME}/moduli" -b 4096
    ssh-keygen -T /etc/ssh/moduli -f "${HOME}/moduli"
    rm "${HOME}/moduli"

#### Client config

    Host *
        KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
        PasswordAuthentication no
        PubkeyAuthentication yes
        Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
        MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-ripemd160-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,hmac-ripemd160,umac-128@openssh.com
    
    # if you use GitHub via SSH uncomment:
    #Host github.com
    #    KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1

Things you'll probably need to run:

    ssh-keygen -t ed25519 -o -a 100
    ssh-keygen -t rsa -b 4096 -o -a 100

#### Summary

You should probably expect some things to break, so do the above carefully.
