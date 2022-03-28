---
title: Laravel | Sync Data From an Existing Database
date: 2022-03-17 09:29:18
categories: [Tech, PHP]
tags:
    - laravel
    - database
---

## Intro

I got a mission to clone an existing service, which has its database separately from the original.

Here is my to-do list:
1. create tables
2. sync data

This article will focus on how to sync data. To read how to create tables from the old database check out [here](/laravel-migration-from-an-existing-database)

## Laravel Database Sync

I found this great tool to do this task: https://github.com/waelwalid/laravel-database-sync

Please follow the instructions to install the tool.

## Adjustments

I run my app in a php-fpm-alpine docker container so some adjustments need to do or the tool won't work.

1. Make the container can run the shell scripts
2. Install mysql-client
3. Disable LOCK TABLE if you are NOT a root user in the synced database when executing mysqldump

### 1. Make the container can run the shell scripts
The sync-tool is implemented by shell scripts, and with the header, `#!/bin/bash`, so only bash can run it.

And since I used an alpine image, which doesn't have the Bash installed by default so I had two options:
1. Change the header to `#!/bin/sh`
2. Install Bash or just use an Ubuntu image for convenience

```
# add this to the Dockerfile
RUN apk add --no-cache --upgrade bash
```

### 2. Install mysql-client
Still, by default, the alpine image doesn't come with mysql-client, which is used in the shell script so we need to install it.

```
apk add --no-cache mysql-client
```

### 3. Disable LOCK TABLE if you are NOT a root user in the synced database


Since I sync the data using a user who ONLY has SELECT privilege, I need to disable LOCK TABLE, and PROCESS when dumping the data.

Open `vendor/waelwalid/laravel-database-sync/database-update.sh`.

Add `--single-transaction=TRUE --no-tablespaces` after `mysqldump` command.

Like this: 
```
mysqldump -v --single-transaction=TRUE --no-tablespaces \
-h $REMOTE_DATABASE_HOST -u $REMOTE_DATABASE_USER \
-p $REMOTE_DATABASE_PASS $REMOTE_DATABASE_NAME \
> $CONFIG/dumps/remote-database-$CURRENT_TIME.sql
```

If you use login as root, or other users with LOCK TABLE, and PROCESS privileges, you can skip this step.

Quote from the document:
> mysqldump requires at least the SELECT privilege for dumped tables, SHOW VIEW for dumped views, TRIGGER for dumped triggers, LOCK TABLES if the --single-transaction option is not used, and (as of MySQL 8.0.21) PROCESS if the --no-tablespaces option is not used. Certain options might require other privileges as noted in the option descriptions.
> 
[Check out for more details about mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)

## After all

Now you can run `composer database-update` to sync the data, or just execute `.vendor/waelwalid/laravel-database-sync/database-update.sh` which is the same, cheers :)