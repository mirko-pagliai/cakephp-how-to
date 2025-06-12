## Testing protected/private methods of a trait
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

## Testing `Query`

Suppose we need to write some tests on a query (`SelectQuery`).
These tests, however, only involve methods that, in reality, do not really need a real connection, for example `getValueBinder()` or `setValueBinder()`.

I could use a test connection, which I may already have available, otherwise I could complementary avoid using a connection.

The problem is that the `SelectQuery` constructor requires a table.
Instead of making a mock of `SelectQuery` or using a real `SelectQuery` instance and passing it a mock of the table, I can use a new object that extends `SelectQuery`, but overrides its constructor:

```php
$Query = new class extends SelectQuery {
    public function __construct()
    {
    }
};
$Query->getValueBinder()->bind(':c0', 1, 'integer');
$Query->getValueBinder()->bind(':c1', 'value', 'string');

//Some assertions here...
```
