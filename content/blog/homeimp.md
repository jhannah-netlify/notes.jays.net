---
title: Home Improvement
description: Less carpety
date: 2026-04-11
draft: true
tags:
---

https://mermaid.js.org/syntax/gantt.html

```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title       Home improvement

    section BR2 Office
    Clear                 :s10, 2026-06-01, 5d
    Carpet out            :s20, after s10, 5d
    Paint                 :s25, after s20, 5d
    New flooring          :s30, after s25, 5d
    New desk              :s40, after s30, 5d
    Move office stuff in  :s50, after s40, 5d
```

```mermaid
gantt
    dateFormat  YYYY-MM-DD

    section BR3 Cat Room
    Clear                   :s10, 2026-08-01, 5d
    Carpet out              :s20, after s10, 5d
    New flooring            :s30, after s20, 5d
```

```mermaid
gantt
    dateFormat  YYYY-MM-DD

    section Basement
    Carpet out              :s10, 2026-08-01, 5d
    New flooring            :s20, after s10, 5d
    Move shit in            :s30, after s20, 5d
    Home theatre            :s40, after s30, 5d
```

Light blue concrete: 
https://mobileimages.lowes.com/productimages/ee4e6587-2609-4676-8cd2-cbc194e2a965/44011919.jpg



```
    section Critical tasks
    Completed task in the critical line :crit, done, 2014-01-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :until isadded
    Functionality added                 :milestone, isadded, 2014-01-25, 0d

    section Documentation
    Describe gantt syntax               :active, a1, after des1, 3d
    Add gantt diagram to demo page      :after a1  , 20h
    Add another diagram to demo page    :doc1, after a1  , 48h

    section Last section
    Describe gantt syntax               :after doc1, 3d
    Add gantt diagram to demo page      :20h
    Add another diagram to demo page    :48h
```
