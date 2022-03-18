# REST API Implementation With Laravel and API Testing

## What is API?

API is a method where two or more software can communicate each other.

## What is REST API

REST (Representational State Transfer) API is an architecture that allow server and client to transfer the representation of the state of the resource to the requester or endpoint. The data can be transferred using JSON, XML, or another format but the most popular option is JSON (JavaScript Object Notation). You can watch [this video](https://youtu.be/iiADhChRriM) if you feel don't familiar with JSON format.

## REST API Implementation with Laravel

### Prerequisites

- PHP 8.0+
- Composer v2.0+
- Database and its credentials

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

## Reference

- [API - Wikipedia](https://en.wikipedia.org/wiki/API)
- [What is a REST API? - Red Hat](https://www.redhat.com/en/topics/api/what-is-a-rest-api)
- [Laravel API Tutorial: Building & Testing a RESTful API | Toptal](https://www.toptal.com/laravel/restful-laravel-api-tutorial)
