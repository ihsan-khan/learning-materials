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

```

```php
// Query examples
$users = User::has('posts')->with('posts')->get();
$users = User::has('posts', '>', 3)->with('posts')->get();
$users = User::whereHas('posts', function($query) {
    $query->where('title', 'like', '%right%');  // Fixed query
})->with('posts')->get();
$users = User::doesntHave('posts')->get();  // Fixed method
```