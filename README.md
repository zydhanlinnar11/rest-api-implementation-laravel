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

First, install sqlite and create sqlite file:

```bash
# Change 8.1 according to your php version
sudo apt install php8.1-sqlite3 sqlite3

touch database.sqlite
```

Open your `.env` file and change `DB_CONNECTION` to `sqlite`, then delete `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`:

```text
DB_CONNECTION=sqlite
```

### Creating Migrations and Model

We want to create model and migration for `Developer` table. Fortunately, Laravel provides easy way to make model and migrations with one single command:

```bash
php artisan make:model Developer --migration
```

Then, you will have two new files `app/Models/Developer.php` for the model and `database/migrations/2022_03_18_090902_create_developers_table.php` for the migrations. Note that the timestamp of migration file depends on when you execute the command.

![Created model and migration files](https://media.discordapp.net/attachments/822059316806811651/954305588400103444/unknown.png)

### Migrate table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('developers', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('developers');
    }
};
```

Short explanation:

- `up()` method will be called when we run the migrations
- `down()` method will be called when we rollback the migrations
- `Schema::dropIfExists('developers')` will drop `developers` table from database if exists
- `Schema::create('developers')` will create `developers` table
- `$table->id()` will create incremental primary key with name `id`
- `$table->timestamps()` will add `created_at` and `updated_at` columns to the table

Supposed we want to add `name` and `fav_lang` columns with string type, we can modify the migration to be like this:

```php
Schema::create('developers', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('fav_lang');
    $table->timestamps();
});
```

Then, run the migration:

```bash
php artisan migrate
```

## Creating Controller

To make simple controller for API, we can use default artisan command with `--model` and `--api` arguments:

```bash
php artisan make:controller DeveloperController --api --model=Developer
```

Here is our controller:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Developer;
use Illuminate\Http\Request;

class DeveloperController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  \App\Models\Developer  $developer
     * @return \Illuminate\Http\Response
     */
    public function show(Developer $developer)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Developer  $developer
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Developer $developer)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Models\Developer  $developer
     * @return \Illuminate\Http\Response
     */
    public function destroy(Developer $developer)
    {
        //
    }
}
```

Look, our controller methods have `$developer` argument. Laravel will find record in database automatically for `Developer` table with [Route model binding](https://laravel.com/docs/9.x/routing#route-model-binding).

## Setting Up Routes

Now, open `routes/api.php` and add `DeveloperController` to the route:

```php
Route::apiResource('developers', DeveloperController::class);
```

Now check our route list:

```bash
php artisan route:list
```

![Route list for Developer resource](https://media.discordapp.net/attachments/822059316806811651/954614420791775262/unknown.png)

Short explanations:

- `GET` method for `api/developers` will be handled by `index()` method in `DeveloperController`. This route is for display a listing of the resource.
- `GET` method for `api/developers/{developers}` will be handled by `show()` method in `DeveloperController`. This route is for display specific resource.
- `POST` method for `api/developers` will be handled by `store()` method in `DeveloperController`. This route is for creating new resource.
- `PUT`/`PATCH` method for `api/developers/{developers}` will be handled by `update()` method in `DeveloperController`. This route is for updating specific resource.
- `DELETE` method for `api/developers/{developers}` will be handled by `destroy()` method in `DeveloperController`. This route is for deleting specific resource.

## Reference

- [API - Wikipedia](https://en.wikipedia.org/wiki/API)
- [What is a REST API? - Red Hat](https://www.redhat.com/en/topics/api/what-is-a-rest-api)
- [Laravel API Tutorial: Building & Testing a RESTful API | Toptal](https://www.toptal.com/laravel/restful-laravel-api-tutorial)
