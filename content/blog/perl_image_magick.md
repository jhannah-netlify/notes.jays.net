---
title: Perl Image::Magick on macOS 15.5 (arm64)
description: How to install
date: 2025-06-26
tags: "perl"
---

Every 3 years or so, for a couple hours, I regret I'm not a C programmer. The other 99.99% of the time I thank Dog I'm not. 😉 

Yesterday I tried to `cpanm Image::Magick` on my MacOS 15.5 (arm64)
and it did not go well:

```
$ cpanm Image::Magick
--> Working on Image::Magick
Fetching http://www.cpan.org/authors/id/J/JC/JCRISTY/Image-Magick-7.1.1-28.tar.gz ... OK
Configuring Image-Magick-v7.1.1 ... OK
Building and testing Image-Magick-v7.1.1 ... FAIL
! Installing Image::Magick failed. See /Users/jhannah/.cpanm/work/1750952897.33635/build.log for details. Retry with --force to force install it.
```

I resolved it with a Homebrew install of imagemagick:

```
$ brew install imagemagick
```

And by hand-modifying the `Makefile.PL` of `Image::Magick`:

```
$ cd /Users/jhannah/.cpanm/work/1750952897.33635/Image-Magick-7.1.1
$ vi Makefile.PL
```

Making these changes (I just changed the front `-I`s of these long lines):

```diff
diff Makefile.PL.orig Makefile.PL
164,165c164,165
< my $INC_magick = '-I/usr/local/include/ImageMagick-7 -DMAGICKCORE_HDRI_ENABLE=1 -DMAGICKCORE_QUANTUM_DEPTH=16 -I/usr/include/libxml2 -I"' . $Config{'usrinc'} . '/ImageMagick-7"';
< my $LIBS_magick = '-L/usr/local/lib -lMagickCore-7.Q16HDRI -lm -L' . $Config{'archlib'} . '/CORE';
---
> my $INC_magick = '-I/opt/homebrew/Cellar/imagemagick/7.1.1-47 -I/opt/homebrew/Cellar/imagemagick/7.1.1-47/include/ImageMagick-7 -DMAGICKCORE_HDRI_ENABLE=1 -DMAGICKCORE_QUANTUM_DEPTH=16 -I/usr/include/libxml2 -I"' . $Config{'usrinc'} . '/ImageMagick-7"';
> my $LIBS_magick = '-L/opt/homebrew/Cellar/imagemagick/7.1.1-47/lib -lMagickCore-7.Q16HDRI -lm -L' . $Config{'archlib'} . '/CORE';
```

And then building, installing myself:

```
$ perl Makefile.PL
$ make test
$ make install
```

## Discussion

Why did I have to do that? Because apparently `Image::Magick`'s `Makefile.PL`
is hard-coded to assume ImageMagick has previously been installed in
`/usr/local/include` and `/usr/local/lib`.
But that's not where Homebrew put it. It put it here:

```
$ brew install pkg-config
$ pkg-config --libs ImageMagick
-L/opt/homebrew/Cellar/imagemagick/7.1.1-47/lib -lMagickCore-7.Q16HDRI
```

Questions? (Remember, I'm not a C programmer. 😉) Toot at me.
Perhaps as a reply [to this thread](https://flyovercountry.social/@deafferret/114745820673706891).
