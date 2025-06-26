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

Making these changes (I just changed the front `-I`, `-L` of these long lines):

```diff
$ diff Makefile.PL.orig Makefile.PL
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

Yay! Installed! 🎉

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

## The original error

```
ld: library 'MagickCore-7.Q16HDRI' not found
```

The full gory details:
```
$ cat /Users/jhannah/.cpanm/work/1750952897.33635/build.log
cpanm (App::cpanminus) 1.7048 on perl 5.040002 built for darwin-2level
Work directory is /Users/jhannah/.cpanm/work/1750952897.33635
You have make /usr/bin/make
You have /usr/bin/curl
You have /usr/bin/tar: bsdtar 3.5.3 - libarchive 3.7.4 zlib/1.2.12 liblzma/5.4.3 bz2lib/1.0.8
You have /usr/bin/unzip
Searching Image::Magick () on cpanmetadb ...
--> Working on Image::Magick
Fetching http://www.cpan.org/authors/id/J/JC/JCRISTY/Image-Magick-7.1.1-28.tar.gz
-> OK
Unpacking Image-Magick-7.1.1-28.tar.gz
Entering Image-Magick-7.1.1
Checking configure dependencies from META.json
Checking if you have ExtUtils::MakeMaker 6.58 ... Yes (7.70)
Configuring Image-Magick-v7.1.1
Running Makefile.PL
Checking if your kit is complete...
Looks good
Warning (mostly harmless): No library found for -lMagickCore-7.Q16HDRI
Generating a Unix-style Makefile
Writing Makefile for Image::Magick
Writing MYMETA.yml and MYMETA.json
-> OK
Checking dependencies from MYMETA.json ...
Checking if you have ExtUtils::MakeMaker 0 ... Yes (7.70)
Checking if you have parent 0 ... Yes (0.241)
Building and testing Image-Magick-v7.1.1
cp Magick.pm blib/lib/Image/Magick.pm
AutoSplitting blib/lib/Image/Magick.pm (blib/lib/auto/Image/Magick)
Running Mkbootstrap for Magick ()
chmod 644 "Magick.bs"
"/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/bin/perl" -MExtUtils::Command::MM -e 'cp_nonempty' -- Magick.bs blib/arch/auto/Image/Magick/Magick.bs 644
"/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/bin/perl" "/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/lib/5.40.2/ExtUtils/xsubpp"  -typemap '/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/lib/5.40.2/ExtUtils/typemap' -typemap '/Users/jhannah/.cpanm/work/1750952897.33635/Image-Magick-7.1.1/typemap'  Magick.xs > Magick.xsc
mv Magick.xsc Magick.c
cc -c  -I/usr/local/include/ImageMagick-7 -DMAGICKCORE_HDRI_ENABLE=1 -DMAGICKCORE_QUANTUM_DEPTH=16 -I/usr/include/libxml2 -I"/usr/include/ImageMagick-7" -fno-common -DPERL_DARWIN -mmacosx-version-min=15.5 -DNO_THREAD_SAFE_QUERYLOCALE -DNO_POSIX_2008_LOCALE -fno-strict-aliasing -pipe -fstack-protector-strong -I/usr/local/include -I/usr/include/freetype2 -g -O2 -Wall -pthread -DMAGICKCORE_HDRI_ENABLE=1 -DMAGICKCORE_QUANTUM_DEPTH=16 -Wno-error=implicit-function-declaration -O3   -DVERSION=\"7.1.1\" -DXS_VERSION=\"7.1.1\"  "-I/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/lib/5.40.2/darwin-2level/CORE"  -D_LARGE_FILES=1 -DHAVE_CONFIG_H Magick.c
rm -f blib/arch/auto/Image/Magick/Magick.bundle
LD_RUN_PATH="/usr/lib" cc -Wl,-rpath,"/usr/lib" -L/usr/local/lib -lMagickCore-7.Q16HDRI  -mmacosx-version-min=15.5 -bundle -undefined dynamic_lookup -fstack-protector-strong   Magick.o  -o blib/arch/auto/Image/Magick/Magick.bundle  \
	   -L/usr/local/lib -lm -L/Users/jhannah/perl5/perlbrew/perls/perl-5.40.2/lib/5.40.2/darwin-2level/CORE   \

ld: library 'MagickCore-7.Q16HDRI' not found
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [blib/arch/auto/Image/Magick/Magick.bundle] Error 1
-> FAIL Installing Image::Magick failed. See /Users/jhannah/.cpanm/work/1750952897.33635/build.log for details. Retry with --force to force install it.
```

## Questions?

Remember, I'm not a C programmer. When C is mad I just thrash around ~randomly
until the computer stops yelling at me. 😉 But you can toot questions at me
if you want. Perhaps as a reply
[to this thread](https://flyovercountry.social/@deafferret/114745820673706891).
