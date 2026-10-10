---
title: "perl5 introspection (thanks Moose!)"
date: 2009-06-17
tags: ["perl"]
original_url: https://headrattle.blogspot.com/2009/06/perl5-introspection-thanks-moose.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2009/06/perl5-introspection-thanks-moose.html) on 2009-06-17.*

Many dynamic languages have a way to get a list of all methods for an object built into them.  
  
I don't think perl5 does. But Moose adds it. :)  

    $ cat j.pl
    package X;
    use Moose;
    sub dosomething1 {}
    sub dosomething2 {}
    sub dosomething3 {}

    package main;
    my $x = X->new();
    print join ", ", sort $x->meta->get_all_method_names;
    print "\n";

    $ perl j.pl
    BUILDALL, BUILDARGS, DEMOLISHALL, DESTROY, DOES, does, 
    dosomething1, dosomething2, dosomething3, dump, meta, new

Neato. :)  
  
More info on Moose: <http://moose.perl.org>

## Comments

**[jhthorsen](http://jhthorsen.github.com)** — June 17, 2009 at 10:05 AM

Moose can't do much without the help from perl5:  
  
\$ perl -le'sub foo{};sub bar{};print for grep { \*{\${main::}{\$\_}}{CODE} } keys %{main::}'  
  
...and you would have to traverse @ISA recursively to get all_methods. But by any means: use Moose; \#!:)

