---
title: "Mojo::DOM CSS selectors sure are handy"
date: 2013-10-11
tags: ["css", "mojolicious", "perl"]
original_url: https://deafferret.tumblr.com/post/63743840385/mojodom-css-selectors-sure-are-handy
templateEngineOverride: md
---

*Originally posted to [deafferret](https://deafferret.tumblr.com/post/63743840385/mojodom-css-selectors-sure-are-handy) on 2013-10-11.*

Really enjoying the [Mojo::DOM](https://metacpan.org/module/Mojo::DOM) bit of Mojo, from the [Mojolicious](http://mojolicio.us/) family of software, which I’ve never used before today. Drops nicely into [Test::WWW::Mechanize](https://metacpan.org/module/Test::WWW::Mechanize) :)

The problem: For a series of websites, for a series of item numbers, if there’s a “choose your item” table then make sure there are quantity_input fields too:

The solution:

    my $mech = Test::WWW::Mechanize->new;
    foreach my $base (@base) {
       foreach my $i (@item_nums) {
          $mech->get_ok(item_url($base, $i));
          diag("$i: " . $mech->title);
          my $dom = Mojo::DOM->new($mech->content);
          SKIP: {
             skip "has no 'choose your item' table", 1 unless
                $dom->find('table#chooseYourItem')->size;
             ok($dom->find('input.quantity_input')->size, "has grid qty inputs");
          }
       }
    }

Slick! :)

Luis Roman wrote a [much longer article](http://luisroman.tumblr.com/post/23164097666/scraping-the-dom-with-css3-selectors-and-perl) about this, if you’re interested.
