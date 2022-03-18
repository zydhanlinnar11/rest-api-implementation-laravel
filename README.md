# REST API Implementation With Laravel and API Testing

## What is API?

API is a method where two or more software can communicate each other.

## What is REST API

REST (Representational State Transfer) API is an architecture that allow server and client to transfer the representation of the state of the resource to the requester or endpoint. The data can be transferred using JSON, XML, or another format but the most popular option is JSON (JavaScript Object Notation). You can watch [this video](https://youtu.be/iiADhChRriM) if you feel don't familiar with JSON format.

## REST API Implementation with Laravel

### Prerequisites

- PHP 8.0+
- Composer v2.0+

### Project Initialization

First, let initialize our Laravel project. This should automatically install the latest version of Laravel.

```bash
composer create-project laravel/laravel rest-api-implementation-laravel
cd rest-api-implementation-laravel
```

Then ensure the server can be up and running:

```bash
php artisan serve
```

Then open [http://127.0.0.1:8000](http://127.0.0.1:8000), you should see this sweet Laravel welcome screen:

![Laravel default welcome screen](https://media.discordapp.net/attachments/822059316806811651/954301257496018984/unknown.png?width=1020&height=671)

### Setting Up Database

I want to make this tutorial to be as simple as possible, so we're going to use SQLite3 for our database connection.

First, create sqlite file:

```bash
touch database.sqlite
```

Open your `.env` file and edit this section to connect to your database file:

```text
DB_CONNECTION=sqlite
DB_HOST=/home/zydhanlinnar11/Github/rest-api-implementation-laravel/database.sqlite
```

### Creating Migrations and Model

We want to create model and migration for `Developer` table. Fortunately, Laravel provides easy way to make model and migrations with one single command:

```bash
php artisan make:model Developer --migration
```

Then, you will have two new files `app/Models/Developer.php` for the model and `database/migrations/2022_03_18_090902_create_developers_table.php` for the migrations. Note that the timestamp of migration file depends on when you execute the command.

![Created model and migration files](https://media.discordapp.net/attachments/822059316806811651/954305588400103444/unknown.png)

## Reference

- [API - Wikipedia](https://en.wikipedia.org/wiki/API)
- [What is a REST API? - Red Hat](https://www.redhat.com/en/topics/api/what-is-a-rest-api)
- [Laravel API Tutorial: Building & Testing a RESTful API | Toptal](https://www.toptal.com/laravel/restful-laravel-api-tutorial)
