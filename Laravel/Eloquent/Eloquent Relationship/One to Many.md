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

1. **Custom Foreign/Local Keys**  
   ```php
   return $this->hasMany(Post::class, 'foreign_key', 'local_key');
   ```

2. **Eager Loading with Constraints**  
   ```php
   // Load only published posts
   User::with(['posts' => fn($query) => $query->where('published', true)])->get();
   ```

3. **Advanced Query Filtering**  
   ```php
   // Users with at least 5 posts
   User::has('posts', '>=', 5)->get();

   // Users with posts matching a condition
   User::whereHas('posts', fn($query) => $query->where('views', '>', 1000))->get();
   ```

4. **Relationship Methods**  
   ```php
   // Create related models
   $user->posts()->create(['title' => 'New Post']);

   // Save multiple (avoids N+1 inserts)
   $user->posts()->saveMany([
       new Post(['title' => 'First']),
       new Post(['title' => 'Second'])
   ]);

   // Update all related
   $user->posts()->update(['featured' => true]);
   ```

5. **Counting Without Loading**  
   ```php
   // Efficient count
   $user->posts()->count();

   // Conditional count
   $user->posts()->where('active', true)->count();
   ```

6. **Lazy Eager Loading**  
   ```php
   $users = User::all();
   $users->load('posts'); // Load after initial query
   ```

---

### Common Mistakes to Avoid:
- **N+1 Problem**  
  Always use `with()`:  
  ```php
  // BAD: N+1 queries
  User::all()->each(fn($user) => $user->posts);

  // GOOD: 2 queries
  User::with('posts')->get();
  ```

- **Missing Inverse Relationship**  
  Every `hasMany` should have a `belongsTo`.

- **Database-Level Integrity**  
  Add foreign key constraints in migrations:  
  ```php
  Schema::create('posts', function (Blueprint $table) {
      $table->foreignId('user_id')->constrained()->onDelete('cascade');
  });
  ```

---

### Full Workflow Example:

1. **Migration Setup**  
   ```php
   Schema::create('posts', function (Blueprint $table) {
       $table->foreignId('user_id')->constrained(); // Required for 1:N
       $table->string('title');
       $table->text('body');
   });
   ```

2. **Create with Relationship**  
   ```php
   // Single post
   $user->posts()->create(['title' => 'Hello World']);

   // Multiple posts (efficient)
   $user->posts()->createMany([
       ['title' => 'First'],
       ['title' => 'Second']
   ]);
   ```

3. **Query Patterns**  
   ```php
   // Get all posts with user data (no N+1)
   Post::with('user')->get();

   // Get users with their latest post
   User::with('latestPost')->get();

   // In User model:
   public function latestPost() {
       return $this->hasOne(Post::class)->latestOfMany();
   }
   ```

4. **Delete Strategies**  
   ```php
   // Delete all user's posts
   $user->posts()->delete();

   // Orphan posts (set user_id to null)
   $user->posts()->update(['user_id' => null]);
   ```

---

### Advanced Techniques:
- **Dynamic Relationships**  
  ```php
  // In User model
  public function popularPosts() {
      return $this->hasMany(Post::class)->where('views', '>', 1000);
  }
  ```

- **Touch Parent Timestamps**  
  Automatically update `users.updated_at` when post changes:  
  ```php
  class Post extends Model {
      protected $touches = ['user']; 
  }
  ```

- **Default Ordering**  
  ```php
  public function posts() {
      return $this->hasMany(Post::class)->orderBy('created_at', 'desc');
  }
  ```

---

### Key Differences from One-to-One:
1. **Plural method names** (`posts()` not `post()`)
2. **Multiple records** per parent (vs. single in 1:1)
3. **Bulk operations** (`createMany()`, mass updates)
4. **Aggregates** (`count()`, `sum()`, etc.) are more common
5. **Hierarchical queries** (e.g., nested eager loading)
