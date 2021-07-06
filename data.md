---
layout: default
title: Managing data
nav_order: 4
---

# Managing data
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Folder structure

Generally, the folder structure I like to follow is the following:
```bash
mydataset/
├── metadata
├── processed
├── raw
└── versions
```

* The `raw/` folder can contain the dataset as it is (in whatever structure it originally is in)
* The `processed/` folder can contain a flatlist of data points (e.g. images/text/videos/audios etc) - this is somewhat like a standardisation layer on top of different datasets which can have their own structure internally
* The `versions/` folder may contain the different versions (change a version when the dataset size changes) and splits (different train/val/test slices on the same version)
* The `metadata` folder may contain any other associated metadata, changelog etc.


## Storage

On-prem data storage is the easiest option. The more robust options (but expensive) would be to store data on AWS S3 buckets or EFS (very expensive). `s3fs` is a nice tool to access S3 buckets like a usual file-system.


## Databases

TODO

## Versioning

I've so far followed the naive versioning strategy:
* Make a new version `v(x+1)` from `vx` whenever there is a change to the size of the dataset (typically, adding new samples)
* To maintain splits within version `vx`, you can make `vx.1`, `vx.2` and so on.

Note that this is only applicable to your own datasets - public datasets are almost always versioned already.
