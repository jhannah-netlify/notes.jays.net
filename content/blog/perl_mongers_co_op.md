---
title: "Perl Mongers + co-op"
description: Roughing out Salve J. Nilsen proposal
date: 2026-10-21
tags: "perl"
---

I met with Salve J. Nilsen briefly on 2026-10-21. If I'm understanding our conversation correctly:

European laws are coming where software stacks will need to certify security maintenance, or
businesses will be forced to stop using that software. If no organization offers to attest to
the security of Perl / CPAN, businesses will be forced to stop using Perl.

Salve's idea is to launch a "CPAN co-op" (that name is a placeholder) where a new non-profit,
legally registered worker-owned cooperative can offer to sell attestations of security on Perl
stacks for money. That money can be donated and/or distributed amongst the partnering co-op
organizations -- whatever mechanisms are enacted by votes of the member orgs.

Several Perl Monger groups are already legally registered non-profits.
In the United States, this could mean 501(c)3, "Not-For-Profit Corporation", or a dozen
other things, depending on the state?
Other countries have equivalent registration schemes.

Salve would like me to expand the `perl_mongers.xml`
[DTD](https://github.com/perlorg/www.pm.org/blob/master/perl_mongers.dtd)
with new fields so that Perl Monger groups can add their non-profit details if they have
them and wish to be members of the co-op.

This should be an easy change, once we have a few examples to work with? Something like this?

```xml
  <group id="0" status="active">
    <!-- new york was the first group -->
    <name>NY.pm</name>
    <location>
      <city>New York City</city>
      <state>New York</state>
      <country>United States of America</country>
      ...
    <nonprofit>
      <registration_type>501(c)3</registration_type>
      <registration_id>12-3456789</registration_id>
      <!-- 
        A 501 (c) (3) is a US nonprofit the IRS recognizes as tax-exempt and eligible to
        receive tax-deductible donations. An EIN is the unique nine-digit ID the IRS assigns
        to that organization, formatted like 12-3456789.
      -->
```

## Questions

* 15 years ago when I ran a Nebraska USA registered S-Corp, my EIN number was considered sensitive.
Are they sensitive? If so, they shouldn't be sitting in `perl_mongers.xml`?
* A quick Internet search:
  [New York Perl Mongers](https://www.bizprofile.net/ny/brooklyn/perl-mongers-inc)
  is a "Not-For-Profit Corporation", not a 501(c)3?
  * There are 15 fields there (Company Name, Entity type, Governing Agency...). Do we want some/all
    of those fields in `perl_mongers.xml`? Those fields will vary WILDLY from country to country? 
    Are we going to try to DTD enforce... How much of this? Just free-form? URL to governing agency,
    if that agency has a publicly accessible URL, to verify current registration? Not just a rumor
    we're spreading in an XML file that is probably out of date?
* **Salve wrote:** Would you mind expanding a little on why you pref the term "legally organized" over "well organized"?
  * **Jay wrote:** When I hear the phrase "well organized" that has nothing to do with legal status. A 
  group can be extremely "well organized," with tons of volunteers and leaders working very hard,
  and constant meetings and new users onboarding, but not have legal status. To me, the phrase "legally organized" or "registered non-profit" removes that confusion.
