# <img src="https://raw.githubusercontent.com/tabuna/breadcrumbs/master/logo.svg" width="30" height="30" alt="Laravel Breadcrumbs"> Laravel Breadcrumbs

Breadcrumbs displays a list of links indicating the position of the current page in the whole site hierarchy. For example, breadcrumbs like

> [Home](#) / [Sample Post](#) / Edit

means the user is viewing an edit page for the "Sample Post". He can click on "Sample Post" to view that page, or he can click on "Home" to return to the homepage.


## Getting Started

## Installation

Run this at the command line:
```php
$ composer require tabuna/breadcrumbs
```
This will update `composer.json` and install the package into the `vendor/` directory.

### Base Usage

Now you can define breadcrumbs directly in the route files:

```php
use Tabuna\Breadcrumbs\Trail;

// Home
Route::get('/', fn () => view('home'))
    ->name('home')
    ->breadcrumbs(fn (Trail $trail) =>
        $trail->push('Home', route('home'))
);

// Home > About
Route::get('/about', fn () => view('home'))
    ->name('about')
    ->breadcrumbs(fn (Trail $trail) =>
        $trail->parent('home')->push('About', route('about'))
);
```

You can also define breadcrumbs separately from the route:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Tabuna\Breadcrumbs\Breadcrumbs;
use Tabuna\Breadcrumbs\Trail;

class BreadcrumbsServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Breadcrumbs::for('home', fn (Trail $trail) =>
             $trail->push('Home', route('home'))
        );
    }
}
```

## Output the breadcrumbs

In order to display breadcrumbs on the desired page, simply call:

```php
@foreach (Breadcrumbs::current() as $crumbs)
    @if ($crumbs->url() && !$loop->last)
        <li class="breadcrumb-item">
            <a href="{{ $crumbs->url() }}">
                {{ $crumbs->title() }}
            </a>
        </li>
    @else
        <li class="breadcrumb-item active">
            {{ $crumbs->title() }}
        </li>
    @endif
@endforeach
```

And results in this output:

> [Home](#) / Blog

#### Tests

```bash
php vendor/bin/phpunit 
```

## Donate & Support

If you would like to support development by making a donation you can do so [here](https://www.paypal.me/tabuna/10usd). &#x1F60A;


## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.
