---
title: AWS S3 Mountpoint
description: Local mounting of S3 buckets
date: 2025-01-15
tags:
  - aws
  - work
---

Released in March 2023, this is interesting?

[Working with Mountpoint for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mountpoint.html)

> Shared cache – You can use a shared cache on S3 Express One Zone. If you
repeatedly read small objects from multiple compute instances or if you do not
know the size of your repeatedly read dataset and want to benefit from
elasticity of cache size, you should opt in to the shared cache. Once you opt
in, Mountpoint retains objects with sizes up to one megabyte in a directory
bucket that uses S3 Express One Zone.

> Amazon S3 Express One Zone is a high-performance, single-Availability Zone
storage class purpose-built to deliver consistent single-digit millisecond data
access for your most frequently accessed data and latency-sensitive
applications. S3 Express One Zone delivers data access speed up to 10x faster
and request costs up to 50% lower than S3 Standard. 

Maybe this is a good fit for `$current_work_problem`?
I'll just mount this 2TB into my ECS real quick? 😉
