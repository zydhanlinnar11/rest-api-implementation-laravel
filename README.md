# REST API Implementation With Laravel and API Testing

## What is API?

API is a method where two or more software can communicate each other.

## What is REST API

REST (Representational State Transfer) API is an architecture that allow server and client to transfer the representation of the state of the resource to the requester or endpoint. The data can be transferred using JSON, XML, or another format but the most popular option is JSON (JavaScript Object Notation). You can watch [this video](https://youtu.be/iiADhChRriM) if you feel don't familiar with JSON format.

## REST API Implementation with Laravel

### Prerequisites

- PHP 8.0+
- Composer v2.0+
- [Postman](https://www.postman.com/)

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

## Handling Request

### Creating Resource

When creating new resource, we need to take input `name` and `fav_lang` from user and put it to the database.

```php
public function store(Request $request)
{
    $developer = new Developer();
    $developer->name = $request->input('name');
    $developer->fav_lang = $request->input('fav_lang');
    
    $developer->save();
    return response()->json([ 'message' => 'Resource created' ]);
}
```

#### Test Create Resource API

We're going to make `POST` request to `http://localhost:8000/api/developers` using Postman. First, we need to set `Content-Type` and `Accept` header to `application/json` to make sure we always receive JSON response from Laravel.

![Content-Type and Accept header](https://media.discordapp.net/attachments/822059316806811651/954622760502587392/unknown.png)

Now, we want to insert developer with name `Zydhan Linnar Putra` and his favourite languange which is `C++`:

![Resource successfully created](https://media.discordapp.net/attachments/822059316806811651/954623959771516958/unknown.png?width=622&height=479)

Nice! We received response that we want.

### Listing Resources

We only need to return all records in database using `Developer::all()`:

```php
public function index()
{
    return Developer::all();
}
```

#### Test Resources Listing

To test this functionality, we should make `GET` request to `http://localhost:8000/api/developers`:

![Resource listing Postman](https://media.discordapp.net/attachments/822059316806811651/954624971815149578/unknown.png?width=661&height=479)

### Showing Specific Resource

We only need to return `$developer` from method parameter:

```php
public function show(Developer $developer)
{
    return $developer;
}
```

#### Test Display Specific Resource

We should make `GET` request to `http://localhost:8000/api/developers/{resource_id}`. According to [resources listing](#test-resources-listing) the resource id is 1, so we need to test `http://localhost:8000/api/developers/1`:

![Specific resource Postman](https://media.discordapp.net/attachments/822059316806811651/954626556188303400/unknown.png?width=665&height=479)

### Updating resources

Like [resource creation](#creating-resource), we also need to take input from user, and update it to database. But for updating, we don't need to create new `Developer` instance, we only need to update instance that given in method param:

```php
public function update(Request $request, Developer $developer)
{
    $developer->name = $request->input('name');
    $developer->fav_lang = $request->input('fav_lang');
    
    $developer->save();
    return response()->json([ 'message' => 'Resource updated' ]);
}
```

#### Test Update Specific Resource

Make a `PUT` request to `http://localhost:8000/api/developers/1` with body structure [resource creation](#creating-resource), but in this case we want to change his name to `Zydhan` and make `PHP` as his favourite language:

![Resource update Postman](https://media.discordapp.net/attachments/822059316806811651/954628113575333908/unknown.png?width=655&height=479)

Then, [confirm if the resource is updated](#test-display-specific-resource):

![resource successfully updated Postman](https://media.discordapp.net/attachments/822059316806811651/954628490286739456/unknown.png?width=665&height=479)

If you look at it, Laravel also automatically update `updated_at` column.

### Delete resource

Eloquent provide `delete()` method to delete resource from table:

```php
public function destroy(Developer $developer)
{
    $developer->delete();

    return response()->json([ 'message' => 'Resource deleted' ]);
}
```

#### Test resource deletion

Method for this request is `DELETE` and we want to delete developer with name `Zydhan` which has ID of 1. So we make `DELETE` request to `http://localhost:8000/api/developers/1`:

![Resource deletion Postman](https://media.discordapp.net/attachments/822059316806811651/954629656143876106/unknown.png?width=625&height=479)

Now if we access [resource listing](#listing-resources), we should receive empty array:

![Empty array Postman](https://media.discordapp.net/attachments/822059316806811651/954630039851368478/unknown.png?width=803&height=479)

And if we try to access [with resource ID](#test-display-specific-resource), we got 404 not found:

![Not found Postman](https://media.discordapp.net/attachments/822059316806811651/954630390688145428/unknown.png?width=648&height=479)

## Verdict

Laravel bring us ease of API development with eloquent, API controller, migration, etc. There are many room of improvements here, right now we don't have any authentication method so that random user could access our API and do whatever they want. Let me know if you want i make the tutorial to secure your API!

## Bonus

### API Resources

Typically, you don't want to return all column for listing resources, to achieve it, you can use API Resource to transform eloquent model before returning it.

First, create resource file:

```bash
php artisan make:resource DeveloperResource
```

We only want to return developer ID and its name, so we only put those columns to resource file:

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name
    ];
}
```

Then, in `DeveloperController@index`, we should call resource file when returning it.

```php
public function index()
{
    return DeveloperResource::collection(Developer::all());
}
```

![DeveloperResource testing](https://media.discordapp.net/attachments/822059316806811651/954635360653156412/unknown.png)

Now, it only return `id` and `name`. If we look closer, it also add `data`, that will be useful if you use pagination. You can read more about API resources [here](https://laravel.com/docs/9.x/eloquent-resources).

### Input Validation

What if we don't fill all the input when creating resource? Let's try it:

![Test unfilled input](https://media.discordapp.net/attachments/822059316806811651/954636874142613544/unknown.png?width=662&height=479)

Wow, we got Internal server error. We shouldn't return those error because the problem is from the user. Instead, we should validate data before doing any business logic. First, create request file:

```bash
php artisan make:request StoreDeveloperRequest
```

Now we have new file `app/Http/Requests/StoreDeveloperRequest.php`:

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreDeveloperRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return false;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            //
        ];
    }
}
```

Short explanation:

- `authorize()` method is for checking whether it is authorized to execute that request. For now, set it to `true` to authorize all request.
- `rules()` method is for validating input.

Now, we want to make `name` and `fav_lang` to be required. Let add it to the rules array:

```php
public function rules()
{
    return [
        'name' => 'required',
        'fav_lang' => 'required'
    ];
}
```

Then, in the controller, substitute `Request` with `StoreDeveloperRequest`:

```php
public function store(StoreDeveloperRequest $request)
{
    $developer = new Developer();
    $developer->name = $request->input('name');
    $developer->fav_lang = $request->input('fav_lang');
    
    $developer->save();
    return response()->json([ 'message' => 'Resource created' ]);
}
```

Test it again:

![Test storing unprocessable content](https://media.discordapp.net/attachments/822059316806811651/954639595251839006/unknown.png?width=657&height=479)

Now we got proper 422 - Unprocessable Content error and message that we must fill the `name` and `fav_lang` column. You can read more about another validation rules beside `required` [here](https://laravel.com/docs/9.x/validation#available-validation-rules).

## Reference

- [API - Wikipedia](https://en.wikipedia.org/wiki/API)
- [What is a REST API? - Red Hat](https://www.redhat.com/en/topics/api/what-is-a-rest-api)
- [Laravel API Tutorial: Building & Testing a RESTful API | Toptal](https://www.toptal.com/laravel/restful-laravel-api-tutorial)
