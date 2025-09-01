# IN PROGRESS
# Basic Setup

To create a new laravel app

```php
# FILL IN
```

To run your app

```php
# Start XAMPP then run
php artisan serve
```

# Routing

To create a page:

- create a filename.blade.php file in resources views
- add a new route for that view

```php
# To get a page with no variables passed in
Route::get('/page', function(){
	return view('filename of blade template');
});

# For a dynamic page that renders with id
Route::get('/page/{id}', function($id){
	dd($id); # to see just see what got passed in
	ddd($id); # to see code and line number where thing gets passed in
	return view('viewname/{$id}');
})->where('id', 'regex'); # to validate input, make id only be numbers
```

# Migrations

> **Purpose:** You can't upload your database to github. For everyone to be on the same page you create migrations and seeders. Migrations set up the table structure and seeders populate it with sample data

When you run a migration file tables are created for you. You don't have to go to MySQL to make the tables.

```php
# Run in terminal to create a migration file
php artisan make:migration migration_name
# names like create_tablename_table is appropriate
```

The migration file will be created in database/migrations and by default will have a up and down method. Up is for creating the table and down is for deleting it.

For available data types refer to [this](https://laravel.com/docs/12.x/migrations#available-column-types).

```php
# By default you have this in your add method:
Schema::create('medicine_catalogue', function (Blueprint $table) {

            $table->id();

            $table->timestamps();
           
            # add new columns here with the same format as above
            # put the names of the columns inside brackets like so:
            $table->string('description');
        });
```

To create the tables in the terminal run:

```php
php artisan migrate
```

To clear the contents of your tables run:

```php
php artisan migrate:refresh
```

To refresh and seed (as in clear all tables and then populate it):

```php
php artisan migrate:refresh --seed
```

To clear all existing tables and make migrations run:

```php
php artisan db:wipe
php artisan migrate
```

Check if your tables were created through PHPMyAdmin or other MySQL software.

## Seeders and Factories

To populate the database Laravel uses seeders and factories.

```php
# Inside the run function you may see code like this:
User::factory(10)->create();
# This generates 10 users using a factory
# You may see what is happening in the factory by going to databases/factories
# The file inside facories contain calls to fake() which is a library that generates fake data to populate the database
```

# Eloquent Models

> **Purpose:** Models extend the model class, which come with pre-existing functions to handle queries to the database

Create a new model using:

```php
php artisan make:model modelName
# It is standard practice to create tables using plural terms and models using singular terms
# e.g. for the users table, you create the User model
```

### Default Behaviour

==**Model and Table Names**==
The model **User**, connects to table **users**
The model **Hello_World**, connects to table **hello\_\_worlds**
The model **HelloWorld**, connects to table **hello_worlds**

**==Primary Key==**
The primary key by default is assumed to be a column named 'id'.
It is also expected to be an incrementing integer. I wanted this behavior anyways so I didn't change the default.

**==Expected Columns in Your Table==**
Tables are expected to have a _created_at_ and _updated_at_ column.

To overwrite the default conventions:

```php
# in app/Models/ModelName.php
class ModelName extends Model

{
	# to specify which table
    protected $table = 'tablename';
	# to specify whick PKey.
    protected $primaryKey = 'medicine_id';
    # to specify that there is not created_at and updated_at column
    public $timestamps = false;

}
```

# Layouts

You can use blade for templating. Instead of having to add HTML5 boilerplate to every page, in addition to things like navigation bars that are supposed to be in every page you can create a layout and have your other pages extend that layout. The layout will have a slot defined for where code goes into the template.

This is what your 'base' (the part that you want to take into every page) looks like.

> **==typing ! on vs code and then pressing enter automatically inserts HTML5 Boilerplate==**

```php
// This file exists in resources/views the extension is .blade.php
//
# Pretend
# there's
# a
# bunch
@yield('name_of_your_section');
# of
# html
# here
```

The page that is taking from the base will have:

```php
@extends('name_of_your_base_file')
@section('name_of_your_section')
# your code
@endsection
```

To include parts of a website (lets say a banner):

```php
# Create a folder called partials under resources/views
# It is standard practice to name this files starting with an underscore
# e.g. _nameOfFile.blade.php

@extends('name_of_your_base_file')
@section('name_of_your_section')
# add the partial view
@include('partials._nameOfFile')
# your code
@endsection
```

## Connect to tailwind

Follow instructions [here](https://tailwindcss.com/docs/installation/framework-guides/laravel/vite)

```php
#Include this in the head tag of your HTML
@vite(['resources/css/app.css', 'resources/js/app.js'])

# Add to 'resources/css/app.css'
@import 'tailwindcss';
@import 'tailwindcss/preflight';
@tailwind utilities;

# Run in terminal
npm run dev
npm run build

# then start your application and check if it is working
```

Setup your editor properly. On VS code install:

1. Tailwind CSS IntelliSense extension
2. Install a PostCSS language support extension

## References

1. [Laravel From Scratch - Traversy Media](https://www.youtube.com/watch?v=MYyJ4PuL4pY)
2. [Laravel Crash Course - Hitesh Choudhary](https://www.youtube.com/watch?v=Ea1gVO53lDY)
