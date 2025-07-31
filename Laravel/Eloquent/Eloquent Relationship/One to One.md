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

1. **Custom Foreign Key**  
   If your foreign key isn't `user_id`:
   ```php
   return $this->hasOne(Address::class, 'foreign_key_column');
   ```

2. **Eager Loading (N+1 Prevention)**  
   Always eager load relationships in queries:
   ```php
   $users = User::with('address')->get(); 
   // Single query for all addresses
   ```

3. **Create/Update Safely**
   ```php
   // Create with null check
   if (!$user->address) {
       $user->address()->create(['name' => 'Home']);
   }

   // Update existing
   $user->address()->update(['name' => 'Office']);
   ```

4. **Delete Relationship**
   ```php
   $user->address()->delete();  // Deletes the address record
   // OR (if using soft deletes)
   $user->address()->dissociate()->save();
   ```

5. **Default Values (Fallback)**
   ```php
   return $this->hasOne(Address::class)->withDefault([
       'name' => 'No address saved'
   ]);
   ```

6. **Performance Tip**  
   Use `exists()` to check relationship without loading data:
   ```php
   if ($user->address()->exists()) {
       // Relationship exists
   }
   ```

### Common Mistakes to Avoid:
- Forgetting to define the **inverse relationship** (`belongsTo`)
- Not using `with()` in queries (causes N+1 issues)
- Assuming the relationship exists without null checks (`$user->address?->name`)

### Full Example Workflow:
1. **Migration Setup**  
   ```php
   Schema::create('addresses', function (Blueprint $table) {
       $table->foreignId('user_id')->constrained()->unique(); // 1:1 enforcement
       $table->string('name');
   });
   ```
2. **Retrieving Data**  
   ```php
   $user = User::with('address')->find(1);
   echo $user->address->name; // Safe with withDefault()
   ```
3. **Updating**  
   ```php
   $user->address()->updateOrCreate([], ['name' => 'New Address']);
   ```
```

### Added Critical Missing Parts:
1. **Inverse relationship** (`belongsTo`)  
2. **Custom foreign key** support  
3. **Eager loading** best practices  
4. **Null-safe operations** and existence checks  
5. **Database-level enforcement** via migrations  
6. **UpdateOrCreate** pattern for atomic operations  



