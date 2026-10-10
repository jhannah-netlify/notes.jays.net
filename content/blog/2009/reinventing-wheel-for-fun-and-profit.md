---
title: "Reinventing the wheel for fun and profit"
date: 2009-02-14
tags: ["bio", "perl"]
original_url: https://headrattle.blogspot.com/2009/02/reinventing-wheel-for-fun-and-profit.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2009/02/reinventing-wheel-for-fun-and-profit.html) on 2009-02-14.*

I was all proud of myself last week and impressed the other kids when I wrote an amino acid property converter in about 15 minutes.

    $ perl properties.pl TLPDILHASGPPGPPGQPEQPGAGR
                   small 1011000111111111010011110
                aromatic 0000001000000000000000000
               non_polar 0100111101001001000001110
                    tiny 1000000111001001000001110
                 charged 0001001000000000001000001
               aliphatic 0100110000000000000000000
                positive 0000001000000000000000001
                   polar 1001001010000000101100001

Little did I know that [mine](https://clabsvn.ist.unomaha.edu/anonsvn/user/jhannah/JCVI/Project_C/properties.pl) was inferior to [Bio::Tools::OddCodes](http://search.cpan.org/~cjfields/BioPerl-1.6.0/Bio/Tools/OddCodes.pm), which I could have downloaded for free in about 1 minute.  
  
But, in my defense, not knowing that tool existed it would have taken me longer to find it (not knowing what I was searching for) than to write my own from scratch.  
  
And as a programmer, it's far more fun and satisfying to spend 15 minutes solving a problem and impressing people than to spend 15 minutes searching the Internet for something you may never find.  
  
So perhaps reinventing the wheel is good for morale sometimes. Trivial wheels, at least. :)

## Comments

**Anonymous** — February 14, 2009 at 11:13 AM

Can I come on your defense too? 'OddCodes' is most certainly \*not\* a keyword that I would use to search for a solution to your problem. I found it browsing for something else, and when I read the synopsis I realized how useful of a module it is - and how hidden form everyone due to some obscure naming decision.  
++ for impressing everyone whit Perl's whipuptitude!

