# Laravel Order By Field

This package provides a few Laravel `Query\Buider` marco in order to implement MySQL-like `order by filed(...)` feature.

## Installation

Get the package

```shell
composer require mihai-valentin/laravel-order-by-field
```

## Usage

```php
use \Illuminate\Support\Facades\DB;

// Order records by a column asc
DB::table('table_name')->orderByField('column', ['first', 'second', 'third']);

// Order records by a column desc
DB::table('table_name')->orderByField('column', ['first', 'second', 'third'], 'desc');
DB::table('table_name')->orderByFieldDesc('column', ['first', 'second', 'third']);
```

## How it works

For the MySQL macro will generate a native `order by field(...)` expression. For all other drivers order clause will be
implemented using `case` predicate.

### Using MySQL

```php
use \Illuminate\Support\Facades\DB;

// Before
DB::table('table_name')->orderByRaw("field(`column`, 'first', 'second', 'third')");

// With macro
DB::table('table_name')->orderByField('column', ['first', 'second', 'third']);
```

### Using Postgresql, Sqlite

```php
use \Illuminate\Support\Facades\DB;

// Before
DB::table('table_name')->orderByRaw("
    case
        when \"column\"='first' then 1
        when \"column\"='second' then 2
        when \"column\"='third' then 3
        else 0
    end
");

// With macro
DB::table('table_name')->orderByField('column', ['first', 'second', 'third']);
```

## PhpStorm autocomplete

You can create an `_ide_helper` file to tell your IDE about new methods. The helper file can look like this

```php
<?php

namespace Illuminate\Contracts\Database\Query;

use MihaiValentin\LaravelOrderByFiled\OrderByFieldServiceProvider;

/**
 * @method Builder orderByField(string $column, array $order, string $direction = 'asc')
 * @method Builder orderByFieldDesc(string $column, array $order)
 *
 * @see OrderByFieldServiceProvider
 */
interface Builder {}
```

> You can find
> it [here](https://github.com/mihai-valentin/laravel-order-by-field/blob/97ea045d9fcbc81b3b765bfe1cecec5cbcabce1d/_ide_autocomplete_helper.php)

## PhpStan stub

If you are using `PhpStan` and you want to provide methods meta-data, then you can use a stub

```stub
<?php

namespace Illuminate\Contracts\Database\Query;

/**
 * @method Builder orderByField(string $column, array $order, string $direction = 'asc')
 * @method Builder orderByFieldDesc(string $column, array $order)
 */
interface Builder {}
```

> You can find
> it [here](https://github.com/mihai-valentin/laravel-order-by-field/blob/97ea045d9fcbc81b3b765bfe1cecec5cbcabce1d/.phpstan/stubs/Builder.stub)
>
> Do not forget to add new stub into config `stubFiles` section

## Tests

You can run tests using the `make`

```shell
# Run tests, code static analysis and cs fixer
make test
```

```shell
# Run phpunit tests
make phpunit
```

## Build

You can build the package using the `make`

```shell
# Install composer dependencies and run tests
make
```

## Code of Conduct

In order to ensure that the community is welcoming to all, please review and abide by
the [Code of Conduct](https://github.com/mihai-valentin/laravel-order-by-field/blob/c744134f4e2145138a0d5a15799746708771fcf4/CODE_OF_CONDUCT.md).

## Contributing

Please see [CONTRIBUTING.md](https://github.com/mihai-valentin/laravel-order-by-field/blob/bd7e55ff29a12f05d2f1c751705b98e0a589f096/CONTRIBUTING.md) for details.

## License

The MIT License (MIT). Please
see [License File](https://github.com/mihai-valentin/laravel-order-by-field/blob/2f2de827ddb697e76b1e64a24908ebe3973dce73/LICENSE.md)
for more information.
