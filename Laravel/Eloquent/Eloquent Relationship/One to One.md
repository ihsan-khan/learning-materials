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

## One-to-Many Relationship

```php
// User model
public function addresses() {  // Plural for one-to-many
    return $this->hasMany(Address::class);  // Fixed: hasMany (not hashAny)
}

// Address model
public function user() {  // Fixed method name (not passer)
    return $this->belongsTo(User::class)  // Fixed: belongsTo (not bclongTo)
        ->withDefault([ 
            'name' => 'Guest User'
        ]);
}

// Query examples
$users = User::has('posts')->with('posts')->get();
$users = User::has('posts', '>', 3)->with('posts')->get();
$users = User::whereHas('posts', function($query) {
    $query->where('title', 'like', '%right%');  // Fixed query
})->with('posts')->get();
$users = User::doesntHave('posts')->get();  // Fixed method
```

### Key Corrections:
1. Fixed method names: `hasOne`, `hasMany`, `belongsTo`
2. Corrected relationship naming conventions (plural for one-to-many)
3. Fixed SQL query syntax
4. Proper closure formatting for `whereHas`
5. Removed invalid syntax (`wheredoesnt Have`)
```

