---
title: "Catalyst and Quality Assurance bliss"
date: 2010-05-13
tags: ["perl"]
original_url: https://headrattle.blogspot.com/2010/05/catalyst-and-quality-assurance-bliss.html
templateEngineOverride: md
---

*Originally posted to [headrattle](https://headrattle.blogspot.com/2010/05/catalyst-and-quality-assurance-bliss.html) on 2010-05-13.*

I've fallen in love with website testing. Again. Catalyst and WWW::Mechanize are awesome. The scenario below takes several monotonous and error prone minutes to QA as a human, or several seconds as a series of automated tests. New feature + test coverage == Joy!!  
  
\$work\[0\] test count is back over 3,000 again and [Hudson](http://hudson-ci.org/) is keeping an eye on everything for us every 30 minutes. Very cool.  

    $ perl -Ilib t/avail_book_SG-autoenroll_cancel.t 
    [debug] Statistics enabled
    1..21
    ok 1 - GET http://localhost.omnihotels.com/
    ok 2 - Content-type:
    ok 3 - title>
    ok 4 - Select a City
    ok 5 - No errors
    ok 6 - !- BEGIN: om/rr/RR1_2.tt -->
    ok 7 - !- BEGIN: om/rr/RR1_3.tt -->
    ok 8 - !- BEGIN: om/rr/RR1_4.tt -->
    ok 9 - !- BEGIN: om/rr/RR1_5.tt -->
    ok 10 - Res ID(s) are 40002401426
    ok 11 - SG number 62379458 created by sg_auto_enroll checkbox
    ok 12 - click Activate Account Password & Preferences
    ok 13 - !- BEGIN: om/sg/SI4.tt -->
    ok 14 - submit SI4 (form fields should have been defaulted for us)
    ok 15 - !- BEGIN: om/sg/SI3.tt -->
    ok 16 - submit SI3
    ok 17 - !- BEGIN: om/sg/SG6.tt -->
    ok 18 - GET /?Phoenix_state=clear&pagedst=RR2
    ok 19 - !- BEGIN: om/rr/RR1_5.tt --> Retrieved 40002401426 for cancellation
    ok 20 - Click 'Cancel Reservation'
    ok 21 - Reservation 40002401426 cancelled
    ------------------------------------------------
    $ cat t/avail_book_SG-autoenroll_cancel.t 
    use Test::WWW::Mechanize::Catalyst 'Phoenix';

    use strict;
    use Test::More tests => 21;
    use FindBin qw($Bin);
    use lib "$Bin/lib";
    use phoenix_test;

    my %SI3_args = (
       loginName => ('jhannah' . time),
       password  => 't123',
       EMAIL     => 'jhannah@omnihotels.com',
       Question  => 'CITY',
       Answer    => 'Shmomaha',
    );

    my $mech = Test::WWW::Mechanize::Catalyst->new;
    my $pt = phoenix_test->new(mech => $mech);

    my $cro_resno = $pt->book(sg_auto_enroll => 'on');
    my $msg = qr/Your Select Guest number is (\d+)/;
    my ($sgnum) = ($mech->content =~ $msg);
    ok($sgnum,                                    
       "SG number $sgnum created by sg_auto_enroll checkbox");

    $msg = 'Activate Account Password & Preferences';
    $mech->follow_link_ok({text => $msg},
       "click $msg");

    my $msg = '!- BEGIN: om/sg/SI4.tt -->';
    $mech->content_contains($msg,                 $msg);
    $mech->submit_form_ok({},                     
       "submit SI4 (form fields should have been defaulted for us)");

    my $msg = '!- BEGIN: om/sg/SI3.tt -->';
    $mech->content_contains($msg,                 $msg);
    $mech->submit_form_ok({
       with_fields => \%SI3_args,
    },                                            "submit SI3");

    my $msg = '!- BEGIN: om/sg/SG6.tt -->';
    $mech->content_contains($msg,                 $msg);

    $pt->cancel($cro_resno);

## Comments

**gtermars** — May 13, 2010 at 6:41 PM

What are you using to integrate your Perl testing into Hudson?

---

**Jay** — May 13, 2010 at 6:45 PM

prove --harness=TAP::Harness::JUnit --recurse  
  
Works great. :)

---

**Martin Thomas** — November 13, 2019 at 8:14 AM

NIce Article, Really help me alot... [Google Play Customer Care Numbers](https://www.supports-me.com/2019/11/google-play-customer-care.html) \|\|  
[Top Bidding Site](https://www.bid4jeet.in/pages/schemes) \|\|  
[top Bidding Site Winners](https://www.bid4jeet.in/user/winner) \|\|  
[Amazon Quiz Answers](https://www.freetricksworld.com/2019/11/amazon-quiz-answers-today-amazon-quiz.html)  
[What is my ip](http://whatismyipaddres.net/ip-details)

