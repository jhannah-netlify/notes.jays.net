---
title: "ack is awesome!"
date: 2010-03-04
tags: ["perl"]
original_url: https://headrattle.blogspot.com/2010/03/ack-is-awesome.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2010/03/ack-is-awesome.html) on 2010-03-04.*

Wow! People have been telling me this for years, but I was getting along OK with my old school Unix craziness:

    $ find ./ | grep -v svn | xargs grep '\-\>Promotion'

But today I couldn't get that working. Something with xargs escaping of -\> just refused to fly.  
  
So I installed ack and ran it:

    # cpan App::Ack
    $ ack '\-\>Promotion'

And it found what I was looking for with zero fuss!

    Control/SG.pm
    335:      $self->Promotion($promo_hash);

    View/Web/Phoenix/lib/Phoenix/Controller/SG.pm
    2844:      $c->log->debug("you have these PromotionCode's already: " . (join ", ", keys %{$sg->Promotion}));
    2846:      unless ($sg->Promotion->{$preauth}) {

Holy buckets! That is AWESOME! Thanks Andy!

## Comments

**gregoa** — March 4, 2010 at 6:26 PM

\$ echo \$GREP_OPTIONS  
--exclude-dir=.svn  
\$ mkdir foo  
\$ echo '-\>Promotion' \> foo/bar  
\$ cat foo/bar  
-\>Promotion  
\$ grep -r "\\\>Promotion" .  
foo/bar:-\>Promotion

---

**Unknown** — March 4, 2010 at 8:03 PM

grep -r is nice but you're still going to dig through a lot of files you dont need.  
Ack has a really nice --perl option to filter for \*.pl \*.pm \*.t, saving you some typing.

---

**Brock** — March 5, 2010 at 3:21 PM

You could do:  
  
ack -- '-\>Promotion'  
  
and thus skip the backslashes.

