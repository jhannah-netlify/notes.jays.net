---
title: Castopod
description: My thoughts on Castopod
date: 2025-01-14
tags: "podcast"
---

I've been super into podcasts since before iTunes knew they were a thing.
Big fan. I consume more hours of podcasts than any other form of media.

I love [Mastodon](/blog/mastodon/), and was very enticed by "podcasting
2.0!" It's your podcast, *on the Fediverse!* Oooo! Sold!
So I also publish [my podcast](/blog/jay_flaunts/) on a hosted Castopod
server.

So what's the problem?

Well, for me, the point of being on the Fediverse is full interactions with
others on the Fediverse. I'm Mastodon-centric, so I want full interactions
with Mastodon.

A year ago, as far as I could tell, Castopod <-> Mastodon interactions
were mostly broken.

I'm pleased to report Castopod is *almost* doing what I want now?

Assuming your Castopod server is up to date, you now have
a notification landing page where you can see:

* @foo started following you
* @foo shared your post
* @foo replied to your post

You can click on those notifications and there's a Reply button! Score!

But when you use that Reply button, the person you reply to doesn't receive
a notification on their side.

Oh. Maybe this is just because they've got a trivial bug where they forgot
to stick @foo in the front of the message in their UI?

Oh. Nope. Even if you go out of your way to "@foo hey! thanks! glad you liked
that episode" as your reply, they still don't receive a notification on their
side. It does that weird 

```
@foo@example.com [example.com] hey! thanks!...
```

thing on their side. So they're probably never going to know your podcast
replied to them. Ugh.

(That visual brokenness seems to happen a lot when I try to take the Fediverse
seriously.)

Maybe Castopod is getting super close to being what I want?

If only their hosted server offering (podcast.audio) wasn't insanely slow /
broken ~half the time I try to use it. I only use it a few times a year for a
few minutes, to upload a new episode.

So close to being so awesome??

For example, Castopod is WAY better than my S3 hosting at "scrubbing" --
if people want to listen to your podcast in a browser and instantly jump
30 minutes into the episode by dragging their mouse around, Castopod 
handles that well, while my S3 storage backend just breaks half the time.

(Why would people do that? "Obviously" the right way to listen to podcasts
is to download the entire audio file onto their device in advance
(I love [Pocket Casts](https://pocketcasts.com/) so much), so the
scrubbing happens on their device, not the hosting side. But I have weird
friends in their 20s (gross) who do that nonsense for some reason.)

Which is NOT to say that Castopod might not be exactly what *you* want in
a podcast hosting platform: [castopod.com](https://castopod.com/en)

Maybe I'm just a weirdo. 🙂
