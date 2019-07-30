---
id: 135
title: BackWPup s3 bucket does not exist after selecting it
date: 2017-03-02T00:39:57+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=135
permalink: /backwpup-s3-bucket-does-not-exist-after-selecting-it/
categories:
  - Web Development
  - WordPress
---
This error is not very clear since BackWPup let&#8217;s you select your desired bucket from the list. If the bucket is already selected, how can it not exist? Anyways, I fixed this by switching the region from US-West-Oregon to US-Standard.

<pre>[INFO] BackWPup 3.3.6; A project of Inpsyde GmbH
[INFO] WordPress 4.7.2 on http://example.com/
[INFO] Log Level: Normal 
[INFO] BackWPup job: Weekly Backup
[INFO] Logfile is: backwpup_log_411491_2017-03-01_03-57-39.html
[INFO] Backup file is: backwpup_411491_2017-03-01_03-57-39.tar.bz2
[01-Mar-2017 03:57:39] 1. Try to backup database …
[01-Mar-2017 03:57:39] Connected to database example on localhost
[01-Mar-2017 03:57:39] Added database dump "example.sql.gz" with 290.23 KB to backup file list
[01-Mar-2017 03:57:39] Database backup done!
[01-Mar-2017 03:57:39] 1. Trying to make a list of folders to back up …
[01-Mar-2017 03:57:40] Added "wp-config.php" to backup file list
[01-Mar-2017 03:57:40] 1555 folders to backup.
[01-Mar-2017 03:57:40] 1. Trying to generate a file with installed plugin names …
[01-Mar-2017 03:57:40] Added plugin list file "Example.pluginlist.2017-03-01.txt.bz2" with 1.09 KB to backup file list.
[01-Mar-2017 03:57:40] 1. Trying to generate a manifest file …
[01-Mar-2017 03:57:40] Added manifest.json file with 5.99 KB to backup file list.
[01-Mar-2017 03:57:40] 1. Trying to create backup archive …
[01-Mar-2017 03:57:40] Compressing files as TarBz2. Please be patient, this may take a moment.
[01-Mar-2017 03:58:25] Backup archive created.
[01-Mar-2017 03:58:25] Archive size is 75.79 MB.
[01-Mar-2017 03:58:25] 10163 Files with 159.58 MB in Archive.
[01-Mar-2017 03:58:26] 1. Trying to send backup file to S3 Service …
[01-Mar-2017 03:58:26] ERROR: S3 Bucket "example-backup" does not exist!
[01-Mar-2017 03:58:26] ERROR: Job has ended with errors in 47 seconds. You must resolve the errors for correct execution.
</pre>