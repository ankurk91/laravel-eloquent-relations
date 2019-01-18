# Laravel Eloquent Relationships

[![Packagist](https://img.shields.io/packagist/v/ankurk91/laravel-eloquent-relationships.svg)](https://packagist.org/packages/ankurk91/laravel-eloquent-relationships)
[![GitHub tag](https://img.shields.io/github/tag/ankurk91/laravel-eloquent-relationships.svg)](https://github.com/ankurk91/laravel-eloquent-relationships/releases)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE.txt)
[![Downloads](https://img.shields.io/packagist/dt/ankurk91/laravel-eloquent-relationships.svg)](https://packagist.org/packages/ankurk91/laravel-eloquent-relationships/stats)

This package adds some more relationships to eloquent in Laravel v5.7

## Installation
You can install the package via composer:
```
composer require ankurk91/laravel-eloquent-relationships
```

## Usage
### BelongsToOne
BelongsToOne relation is almost identical to standard BelongsToMany except it returns one model instead of Collection of models 
and `null` if there is no related model in DB (BelongsToMany returns empty Collection in this case). 
Example:
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Ankurk91\Eloquent\BelongsToOne;

class Restaurant extends Model
{
    use BelongsToOne;
    
    /**
     * Each restaurant has only one operator.
     * 
     * @return \Ankurk91\Eloquent\Relations\BelongsToOne
     */
    public function operator()
    {
        return $this->belongsToOne(User::class)          
            ->wherePivot('is_operator', true);
            //->withDefault();
    }

    /**
     * Get all employees including the operator.
     * 
     * @return \Illuminate\Database\Eloquent\Relations\BelongsToMany
     */
    public function employees()
    {
        return $this->belongsToMany(User::class)
            ->withPivot('is_operator');
    }   
}    
```
Now you can access the relationship like:
```php
// eager loading
$restaurant = Restaurant::with('operator')->first();
dump($restaurant->operator);
// lazy loading
$restaurant->load('operator');
// load nested relation
$restaurant->load('operator.profile');
```

## Security
If you discover any security related issues, please email `pro.ankurk1[at]gmail[dot]com` instead of using the issue tracker.

## Attribution
Most of the code is taken from this [PR](https://github.com/laravel/framework/pull/25083)

## License
The [MIT](https://opensource.org/licenses/MIT) License.