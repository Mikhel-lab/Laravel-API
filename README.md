# Laravel-API

Build Eloquent queries from API requests
Latest Version on Packagist Test Status Code Style Status Total Downloads

This package allows you to filter, sort and include eloquent relations based on a request. The QueryBuilder used in this package extends Laravel's default Eloquent builder. This means all your favorite methods and macros are still available. Query parameter names follow the JSON API specification as closely as possible.

Basic usage
Filter a query based on a request: /users?filter[name]=John:
use Spatie\QueryBuilder\QueryBuilder;

$users = QueryBuilder::for(User::class)
    ->allowedFilters('name')
    ->get();

// all `User`s that contain the string "John" in their name
Read more about filtering features like: partial filters, exact filters, scope filters, custom filters, ignored values, default filter values, ...

Including relations based on a request: /users?include=posts:
$users = QueryBuilder::for(User::class)
    ->allowedIncludes('posts')
    ->get();

// all `User`s with their `posts` loaded
Read more about include features like: including nested relationships, including relationship count, custom includes, ...

Sorting a query based on a request: /users?sort=id:
$users = QueryBuilder::for(User::class)
    ->allowedSorts('id')
    ->get();

// all `User`s sorted by ascending id
Read more about sorting features like: custom sorts, sort direction, ...

Works together nicely with existing queries:
$query = User::where('active', true);

$userQuery = QueryBuilder::for($query) // start from an existing Builder instance
    ->withTrashed() // use your existing scopes
    ->allowedIncludes('posts', 'permissions')
    ->where('score', '>', 42); // chain on any of Laravel's query builder methods
Selecting fields for a query: /users?fields[users]=id,email
$users = QueryBuilder::for(User::class)
    ->allowedFields(['id', 'email'])
    ->get();

// the fetched `User`s will only have their id & email set
