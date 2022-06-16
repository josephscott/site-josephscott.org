---
title: PHP Testing With Pest
description: None
date: 2022-06-16
tags:
  - php
  - phpunit
  - pest
---

When I started work on [Marmerine](https://github.com/josephscott/marmerine) I was going to reach for [PHPUnit](https://phpunit.de/) for testing.  Then I came across [Pest](https://pestphp.com/).  I really like the minimal approach to writing tests.

For example, here is one test for the Memcached set command:

```php
test( 'set', function() {
	$key = 'thing';
	$value = 'abc';

	$result = MC::$mc->set( $key, $value );
	$this->assertEquals( true, $result );

	$result = MC::$mc->get( $key );
	$this->assertEquals( $value, $result );
} );
```

The MC class is included as part of the [Pest setup](https://github.com/josephscott/marmerine/blob/trunk/tests/pest.php).

Pest is built on top of PHPUnit, so a lot of this might feel like syntatic sugar on top of an existing solid base.  That is fine with me.
