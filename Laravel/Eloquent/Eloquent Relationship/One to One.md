# Laravel Eloquent Relationships

## One-to-One Relationship

```php
// User model
public function address() {
    return $this->hasOne(Address::class);
}

// Usage
$user->address->name;
```

```sql
SELECT * FROM addresses WHERE user_id = 1;
```

```php
// Creating relationship
$user->address()->create([
    'name' => $request->name
]);

// Alternative creation
$address = new Address([
    'name' => $request->name
]);
$address->user()->associate($user);
$address->save();
```



