---
title: Getting Started With Laravel
---
## Getting Started
Below are the steps required for installing the laravel cli tool and creating a new project

```bash
sudo pacman -S composer
composer global require laravel/installer
export PATH=$PATH:~/.config/composer/vendor/bin/ >> ~/.bashrc
laravel new project-name
```

## Running Your Application
Once in the project folder simply type the following command and visit localhost:8000 in browser

```bash
php artisan serve
```

## Laravel's The MVC Architecture

### Model
Each database table has a corresponding “Model” that allows us to interact with that table. Models give you the way to retrieve, insert, delete and update information into your data table. Models are located in the app directory and simply have a php extension.
Your model name should be the name of the table name minus the s. For example you would have a users table in your database so the model would be called user.
```bash
php artisan make:model ModelName
```

### View
The view files are html responsible for the layout of your applications. Are named with .blade.php extension. These files are located in resources/views/PostView.blade.php No artisan command yet to create views just duplicate the welcome one.
There is no artisan command for generating views yet :(

### Controller
Controllers are responsible for the logic. This is where you write your code for manipulating database stuff, saving files and so on...
```bash
php artisan make:controller ControllerName
```

## Environment File (.env)
The .env file is responsible for your database setup, keys, mail handling and much more. The content below is a snippet from the env file showing how a database would be connected to. Note when using docker the `DB_HOST` should be the name of the docker network (for example db)

```txt
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=password
```

## Migrations
Migrations are files you can create that define a database table structures. Running migrate will create tables in the database defined in your .env file. The point of migrations are so that you don’t have to send colleagues sql code/files.
Running the following command will run all migrations
```bash
php artisan migrate
```
To generate a migration file use the following command.
```bash
php artisan make:migration add_table_name
```
Migrations are located in `./database/migrations/` folder. This is an example of one.
The up function is run when you do a `php artisan migrate`
```php
public function up()
{
    Schema::create(‘test’, function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('airline');
        $table->timestamps();
    });
}
```
The down function is run when you do a `php artisan migrate:rollback`
```php
public function down()
{
    Schema::drop('test');
}
```

## Adding Vuejs and Bootstrap to Your App
Use the following commands below to set up vuejs and bootstrap
```bash
Composer require laravel/ui
Php artisan ui vue
Php artisan ui bootstrap
npm install && npm run dev
```
Place the following link and script tags in your primary page e.g. welcome.blade.php
```html
<link rel="stylesheet" href="{{ asset('css/app.css') }}"> //bootstrap
<script src="{{ asset('js/app.js') }}"></script> //vuejs
```
