# JSON-LD Generator

This a fork of [Torann's JSON-LD generator](https://github.com/Torann/json-ld), I've only extended it with missing things :smile:

Extremely simple JSON-LD generator.

## Installation

- [JSON-LD Generator on Packagist](https://packagist.org/packages/blackfyre/json-ld)
- [JSON-LD Generator on GitHub](https://github.com/blackfyre/json-ld)

From the command line run

```
$ composer require blackfyre/json-ld
```

## Methods

 **/JsonLd/Context.php**

 - `create($context, array $data = [])`
 - `getProperties()`
 - `generate()`

## Context Types

- article
- beach
- blog_posting
- breadcrumb_list
- contact_point
- corporation
- creative_work
- duration
- event
- geo_coordinates
- image_object
- invoice
- job_posting
- list_item
- local_business
- music_album
- music_group
- music_playlist
- music_recording
- news_article
- offer
- order
- organization
- person
- place
- postal_address
- price_specification
- product
- rating
- review
- search_box
- thing
- video_object
- web_page

## Examples

### Quick Example

#### Business

```php
$context = \JsonLd\Context::create('local_business', [
    'name' => 'Consectetur Adipiscing',
    'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor',
    'telephone' => '555-555-5555',
    'openingHours' => 'mon,tue,fri',
    'address' => [
        'streetAddress' => '112 Apple St.',
        'addressLocality' => 'Hamden',
        'addressRegion' => 'CT',
        'postalCode' => '06514',
    ],
    'geo' => [
        'latitude' => '41.3958333',
        'longitude' => '-72.8972222',
    ],
]);

echo $context; // Will output the script tag
```

### News Article

```php
$context = \JsonLd\Context::create('news_article', [
    'headline' => 'Article headline',
    'description' => 'A most wonderful article',
    'mainEntityOfPage' => [
        'url' => 'https://google.com/article',
    ],
    'image' => [
        'url' => 'https://google.com/thumbnail1.jpg',
        'height' => 800,
        'width' => 800,
    ],
    'datePublished' => '2015-02-05T08:00:00+08:00',
    'dateModified' => '2015-02-05T09:20:00+08:00',
    'author' => [
        'name' => 'John Doe',
    ],
    'publisher' => [
        'name' => 'Google',
        'logo' => [
          'url' => 'https://google.com/logo.jpg',
          'width' => 600,
          'height' => 60,
        ]
    ],
]);

echo $context; // Will output the script tag
```


### Using the JSON-LD in a Laracasts Presenter

Even though this example shows using the JSON-LD inside of a `Laracasts\Presenter` presenter, Laravel is not required for this package.

#### /App/Presenters/BusinessPresenter.php

```php
<?php

namespace App\Presenters;

use JsonLd\Context;
use Laracasts\Presenter\Presenter;

class BusinessPresenter extends Presenter
{
    /**
     * Create JSON-LD object.
     *
     * @return \JsonLd\Context
     */
    public function jsonLd()
    {
        return Context::create('local_business', [
            'name' => $this->entity->name,
            'description' => $this->entity->description,
            'telephone' => $this->entity->telephone,
            'openingHours' => 'mon,tue,fri',
            'address' => [
                'streetAddress' => $this->entity->address,
                'addressLocality' => $this->entity->city,
                'addressRegion' => $this->entity->state,
                'postalCode' => $this->entity->postalCode,
            ],
            'geo' => [
                'latitude' => $this->entity->location->lat,
                'longitude' => $this->entity->location->lng,
            ],
        ]);
    }
}
```

#### Generate the Tags

The generator comes with a `__toString` method that will automatically generate the correct script tags when displayed as a string.

**Inside of a Laravel View**

```php
echo $business->present()->jsonLd();
```

**Inside of a Laravel View**

```
{!! $business->present()->jsonLd() !!}
```

## Custom Context Type

The first argument for the `create($context, array $data = [])` method also accepts class names. This is helpful for custom context types.

```php
<?php

namespace App\JsonLd;

use JsonLd\ContextTypes\AbstractContext;

class FooBar extends AbstractContext
{
    /**
     * Property structure
     *
     * @var array
     */
    protected $structure = [
        'name' => null,
        'description' => null,
        'image' => null,
        'url' => null,
    ];
}
```

```php
$context = \JsonLd\Context::create(\App\JsonLd\FooBar::class, [
    'name' => 'Foo Foo headline',
    'description' => 'Bar bar article description',
    'url' => 'http://google.com',
]);

echo $context; // Will output the script tag
```

## Change Log

See the releases section: https://github.com/blackfyre/json-ld/releases
