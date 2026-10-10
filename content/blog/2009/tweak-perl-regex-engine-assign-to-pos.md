---
title: "Tweak the Perl regex engine: assign to pos()"
date: 2009-05-03
tags: ["bio", "perl"]
original_url: https://headrattle.blogspot.com/2009/05/tweak-perl-regex-engine-assign-to-pos.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2009/05/tweak-perl-regex-engine-assign-to-pos.html) on 2009-05-03.*

OK, Perl is way too cool.  
  
I was minding my own business, searching for every occurrence of 'CCAGC' in E-coli, when I hit a snag. Several hundred of my known locations weren't showing up.  
  
Why? Because the Perl regular expression engine, by default, starts searching for the next occurrence of something after the end of the occurrence it just found. This is what most humans want. But you may notice that in the string 'CCAGCCAGC' the thing I'm searching for ('CCAGC') **overlaps itself**, so the regex engine doesn't see the second one.  
  
"Crap," I thought.  
  
But this is Perl -- maybe there's a way? 30 seconds in the documentation (`perldoc -f pos`) and it said I could **assign** to pos(). Really? Sweet! Problem solved!  

    #!/usr/bin/perl

    use strict;

    open (IN, "E_coli.seq");
    my $seq = <IN>;
    chomp $seq;
    close IN;

    my $find_this = 'CCAGC';
    while ($seq =~ /$find_this/g) { 
       my $start = pos($seq) - length( $find_this ) + 1;
       my $stop  = pos($seq);
       pos($seq) = $start;
       print "   Found $find_this at [$start..$stop]\n";
    }

## Comments

**[Matt S Trout](http://shadowcat.co.uk/blog/matt-s-trout)** — May 5, 2009 at 6:51 AM

I'm pretty sure that given the fixed string length, a zero width positive lookahead assertion would do the trick as well. I have absolutely no idea which one would be faster though - that's probably a Benchmark.pm job ...

---

**Nicky Haflinger** — May 6, 2009 at 7:25 AM

Actually the fixed width thing only matters for look behind asserts. Look ahead can contain arbitrary regex though I've never tried putting a zero width assert inside another zero width assert. From personal experience I'd say that a forward assert would be faster then this. I don't know how a look behind would compare.

