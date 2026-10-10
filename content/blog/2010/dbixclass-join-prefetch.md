---
title: "DBIx::Class join, prefetch"
date: 2010-08-13
tags: ["perl"]
original_url: https://headrattle.blogspot.com/2010/08/dbixclass-join-prefetch.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2010/08/dbixclass-join-prefetch.html) on 2010-08-13.*

Over the last 15 years I've joined two database tables into a one-to-many relationship about a billion times. But I've never had any software let me loop the left table and right tables separately before. Turns out this magic has been in DBIx::Class for several years.  
  
[Runnable demo](http://github.com/jhannah/sandbox/tree/master/DBIC_prefetch).  
  
When table 'foo' is in a one-to-many relationship with table 'bar', you can:

    my $foo_rs = $schema->resultset('Foo')->search(
       {},
       {
          join     => 'bars',
          prefetch => 'bars',
          order_by => 'me.id',
       }
    );

    while (my $foo = $foo_rs->next) {
       printf "%s\n", $foo->id;
       if (my $bar_rs = $foo->bars) {
          while (my $bar = $bar_rs->next) {
             printf "   %s\n", $bar->desc;
          }
       }
    }

Output:

    1
    2
       aaa
       bbb
       ccc
    3

Still one SQL statement against the database (fast!). Much prettier than ugly manual de-duping of redundant columns.

## Comments

**mo** — August 13, 2010 at 5:19 PM

actually the join =\> "xxx" is optional

---

**[Htbaa](http://blog.htbaa.com)** — August 14, 2010 at 4:15 AM

Yeah prefetch is magic. One of the things that makes DBIx::Class one of the best ORM's for me.

