# Useful Mercurial settings for working with flies

## Good Tutorials

- [- [http://mercurial.selenic.com/wiki/Tutorial](http://hginit.com/top/])

## Installing on Fedora

    yum install mercurial mercurial-hgk

## Mercurial settings

IMPORTANT: edit this file with your own username!

Create a file {{{$HOME/.hgrc}}} with content similar to

    [defaults]
    
    [ui]
    # this will be the name shown in the commit history
    username = My name <myemail@example.com>
    
    [paths]
    # you can add shortcuts to commonly used repositories here
    flies-upstream=https://@flies.googlecode.com/hg/
    
    [extensions]
    hgk=
    color=
    purge=
    churn=
    fetch=
    
    [diff]
    git = 1
    
    [auth]
    # this wildcard won't work... yet.  http://mercurial.selenic.com/bts/issue1952
    gc.prefix = *.googlecode.com
    gc.username = googleUsername
    gc.password = googlecodePassword
    
    fgc.prefix = flies.googlecode.com
    fgc.username = googleUsername
    fgc.password = googlecodePassword