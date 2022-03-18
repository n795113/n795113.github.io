---
title: Laravel | Migration From an Existing Database
date: 2022-03-17 08:49:12
categories: Tech
tags: 
    - laravel 
    - database
---

## Intro

I got a mission to clone an existing service, which has its database separately from the original.

Here is my to-do list:
1. create tables
2. sync data

This article will only record the first step. the next step will be written in the second episode.

## How to create tables from the existing database
Since this project is a Laravel app, but without migration files so I came out with 2 solutions:
1. Create migrations from the existing database, then run `php artisan migrate` to create the same tables to my new database
2. Create create-table-SQLs from the existing database, then somehow run these SQLs

### Migration Generator
I thought the first one is more ideal since it is easier for other developers to maintain so I googled "laravel create migration ..." then google returned me a bunch of packages to do these tasks, nice!

here is a couple of choices:
- composer package:
    - https://github.com/Xethron/migrations-generator
    - https://github.com/barryvdh/laravel-migration-generator
    - https://github.com/adamkearsley/laravel-convert-migrations
- online convertor
    - https://laravelarticle.com/laravel-migration-generator-online

So I generated all the migrations in seconds, lovely!
Then I run `php artisan migrate`!

CRASH!

Checked out the migrations, found that the generator didn't generate the codes properly. It missed some details, like lacking index, so I needed to go through each file to debug... which was not my purpose to use a tool so I tried the second solution, create tables by pure SQL

### How to Execute SQL Files in Laravel
It is easier than I thought
```php
use Illuminate\Support\Facades\DB;

DB::unprepared(file_get_contents('path/to/sql.sql'));
```
I test a table and it worked properly, created exactly same table as the original without error

## Implement
So I exported the table structure in SQL from phpMyAdmin manually (the total number of tables is still acceptable for me to do this).

Instead of creating a command to do this task, I still generated migrations matched to the tables since I would like to have the ROLLBACK feature in Laravel migration. Remember? We can create migrations in seconds by using one of the mentioned packages. Don't forget to edit `.env` to switch to the original database, And after generating, switch back to the NEW database.

Here is my migration code. I put all the SQL in `database/sql_migrations` but you can put them anywhere you like.
```php=
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;

class CreateNewTable extends Migration {

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        $sqlpath = 'database/sql_migrations/'.basename(__FILE__, ".php").'.sql';
            Illuminate\Support\Facades\DB::unprepared(file_get_contents($sqlpath));
    }


    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('new');
    }

}
```

After all files are set, we can easily run `php artisan migrate` to create tables, and `php artisan migrate:rollback` to rollback.

[Laravel | Sync Data From an Existing Database](/laravel-sync-data-from-an-existing-database)