# Testing protected or private methods of a trait

I need to test the protected or private method `myNonPublic Method()` of my trait `MyTrait`:
```php
trait MyTrait
{
    protected function myNoPublicMethod()
    {
        // ...
    }
}
```

I can create an object (preferably an instance that will actually use that test), which will use that trait, but which overrides the method's visibility:
```php
public function testForMyNoPublicMethod(): void
{
    $object = new class {
        use MyTrait {
            myNoPublicMethod as public;
        }
    };

    // Now I can call the `$object->myNoPublicMethod()` method and make assertions about its result
}
```
