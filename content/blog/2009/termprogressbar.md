---
title: "Term::ProgressBar"
date: 2009-10-08
tags: ["perl"]
original_url: https://headrattle.blogspot.com/2009/10/termprogressbar.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2009/10/termprogressbar.html) on 2009-10-08.*

Here's a super easy CPAN module I started using last week. Ever have a long running process that a human is watching, and you want to add a progress bar? Here you go!  

    use Term::ProgressBar;
    my $progress = Term::ProgressBar->new({
       name  => 'MySlowThing()',
       count => 100,
       ETA   => 'linear', 
    });
    $progress->max_update_rate(1);

    for (1..100) {
       sleep 1;
       $progress->update($_);
    }

  
Gives you a nifty ASCII timer bar with major and minor movements, updating clock, the whole nine. :)  

    MySlowThing():  26% [====        *     ]1m16s Left

## Comments

**Ranguard** — October 8, 2009 at 3:08 PM

Check out: Term::ProgressBar::Simple  
  
That's even... can you guess.. :) ?

---

**[Adam Sjøgren](http://koldfront.dk/)** — October 9, 2009 at 6:40 AM

Looks great!  
  
I wonder if it would be much work to make it resize its output upon SIGWINCH...

