# A fluent builder Schema.org types and ld+json generator

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/schema-org.svg?style=flat-square)](https://packagist.org/packages/spatie/schema-org)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/spatie/schema-org/master.svg?style=flat-square)](https://travis-ci.org/spatie/schema-org)
[![SensioLabsInsight](https://img.shields.io/sensiolabs/i/3af8c447-6f45-40e8-86cf-744fefd5b080.svg?style=flat-square)](https://insight.sensiolabs.com/projects/3af8c447-6f45-40e8-86cf-744fefd5b080)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/schema-org.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/schema-org)
[![StyleCI](https://styleci.io/repos/74684096/shield?branch=master)](https://styleci.io/repos/74684096)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/schema-org.svg?style=flat-square)](https://packagist.org/packages/spatie/schema-org)

`spatie/schema-org` provides a fluent builder for **all** Schema.org types and their properties. The code in `src` is generated from Schema.org's [RFDa standards file](https://github.com/schemaorg/schemaorg/blob/sdo-callisto/data/schema.rdfa), so it provides objects and methods for the entire core vocabulary. The classes and methods are also fully documented as a quick reference.

```php
use Spatie\SchemaOrg\Schema;

$localBusiness = Schema::localBusiness()
    ->name('Spatie')
    ->email('info@spatie.be')
    ->contactPoint(Schema::contactPoint()->areaServed('Worldwide'));

$localBusiness->toScript();
```

```html
<script type="application/ld+json">
{
    "@context": "http:\/\/schema.org",
    "@type": "LocalBusiness",
    "name": "Spatie",
    "email": "info@spatie.be",
    "contactPoint": {
        "@type": "ContactPoint",
        "areaServed": "Worldwide"
    }
}
</script>
```

## Postcardware

You're free to use this package (it's [MIT-licensed](LICENSE.md)), but if it makes it to your production environment we highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

The best postcards will get published on the open source page on our website.

## Installation

You can install the package via composer:

``` bash
composer require spatie/schema-org
```

## Usage

All types can be instantiated though the `Spatie\SchemaOrg\Schema` factory class, or with the `new` keyword.

``` php
$localBusiness = Schema::localBusiness()->name('Spatie');

// Is equivalent to:

$localBusiness = new LocalBusiness();
$localBusiness->name('Spatie');
```

Types can be converted to an array or rendered to a script.

```php
$localBusiness->toArray();

$localBusiness->toScript();

echo $localBusiness; // Same output as `toScript()`
```

There's no full API documentation for types and properties. You can refer to [the source](https://github.com/spatie/schema-org/tree/master/src) or to [the schema.org website](http://schema.org).

If you don't want to break the chain of a large schema object, you can use the `if` method to conditionally modify the schema.

```php
use Spatie\SchemaOrg\LocalBusiness;
use Spatie\SchemaOrg\Schema;

$business = ['name' => 'Spatie'];

$localBusiness = Schema::localBusiness()
    ->name($business['name'])
    ->if(isset($business['email']), function (LocalBusiness $schema) {
        $schema->email($business['email']);
    });
```

> I recommended double checking your structured data with [Google's structured data testing tool](https://search.google.com/structured-data/testing-tool)

### Advanced usage

If you'd need to set a custom property, you can use the `setProperty` method.

```php
$localBusiness->setProperty('foo', 'bar');
```

If you'd need to retrieve a property, you can use the `getProperty` method. You can optionally pass in a second parameter to provide a default value.

```php
$localBusiness->getProperty('name'); // 'Spatie'
$localBusiness->getProperty('bar'); // null
$localBusiness->getProperty('bar', 'baz'); // 'baz'
```

All properties can be retrieved as an array with the `getProperties` method.

```php
$localBusiness->getProperties(); // ['name' => 'Spatie', ...]
```

Context and type can be retrieved with the `getContext` and `getType` methods.

```php
$localBusiness->getContext(); // 'http://schema.org'
$localBusiness->getType(); // 'LocalBusiness'
```

## Known Issues

### Type inheritance

The spec rdfa document that's used to generate this code uses single inheritance for the types. However, the spec on http://schema.org uses multiple inheritance in some cases. Read the docs and use [Google's structured data testing tool](https://search.google.com/structured-data/testing-tool) to ensure you're on the right track!

For example, according to the rdfa, a `LocalBusiness` inherits properties from `Organization`. However, if you visit the spec page on [Schema.org](https://schema.org/LocalBusiness), it inherits properties from `Organization` and `Place`. The current solution is by manually specifying properties on the item, as described above in [advanced usage](#advanced-usage).

```php
Schema::localBusiness()

    // `address` is part of `Organization`, so the method exists
    ->address(/* ... */)

    // `openingHoursSpecification` is part of `Place`, so we need to manually add it
    ->setProperty('openingHoursSpecification', /* ... */);
```

### Other minor issues

- Some docblocks have some formatting issues (PR's welcome!)
- The `Float` type isn't available since it's a reserved keyword in PHP
- The `Physician` type isn't available since it extends a type from the `health` extension spec

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Credits

- [Sebastian De Deyne](https://github.com/sebastiandedeyne)
- [All Contributors](../../contributors)

## About Spatie
Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
