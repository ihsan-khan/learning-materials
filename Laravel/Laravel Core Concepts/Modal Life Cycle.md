# Laravel Model Life Cycle

The Laravel model life cycle refers to the sequence of events that occur when interacting with Eloquent models. Understanding this life cycle is crucial for effectively working with models in Laravel applications.

## Key Life Cycle Events

### 1. Instantiation
When you create a new model instance:
```php
$user = new User();
```
- The model's constructor is called
- The model's `boot()` method is called (if using traits)
- Any global scopes are applied

### 2. Hydration
When you retrieve a model from the database:
```php
$user = User::find(1);
```
- Database query is executed
- Results are hydrated into a model instance
- `retrieved` event is fired

### 3. Attribute Manipulation
When you set attributes:
```php
$user->name = 'John Doe';
```
- The `setAttribute` method is called
- Mutators are applied (if defined)
- Attribute casting is processed

### 4. Saving Process
When you call `save()`:
```php
$user->save();
```

#### a. Before Save
- `saving` event is fired
- `creating` or `updating` event is fired (depending on context)
- Model's `performInsert()` or `performUpdate()` is called

#### b. Database Operation
- SQL query is generated and executed
- Timestamps are updated (if enabled)
- Increment/decrement operations are processed

#### c. After Save
- `created` or `updated` event is fired
- `saved` event is fired
- Relationships are synced (if any)

### 5. Deletion
When you call `delete()`:
```php
$user->delete();
```
- `deleting` event is fired
- Database delete operation occurs
- `deleted` event is fired

### 6. Restoration (Soft Deletes)
If using soft deletes and restoring:
```php
$user->restore();
```
- `restoring` event is fired
- Database update occurs
- `restored` event is fired

## Important Model Methods in the Life Cycle

1. `boot()` - Called when the model is initialized
2. `bootTraits()` - Boots any traits used by the model
3. `fill()` - Hydrates the model with attributes
4. `save()` - Main method for persisting changes
5. `performInsert()`/`performUpdate()` - Handles database operations
6. `delete()` - Handles model deletion

## Observers and Events

You can hook into these life cycle events using:
- Model observers
- Event listeners
- Overriding model methods

Example observer:
```php
class UserObserver
{
    public function creating(User $user)
    {
        // Logic before user is created
    }
    
    public function saved(User $user)
    {
        // Logic after user is saved
    }
}
```

Understanding this life cycle helps you properly extend model behavior and hook into the right points for custom logic.

# Lavavel model
## Life Cycle Event

- **model**  
  lifecycle events allow you execute code during model lifecycle event.  

- **Refined**  
  Creating (3) created (5) saving (6) saved (7) updating (8) updated (9) deleting (10) deleted (11) restoring (12) restored.  

---

1. Refined event will be fixed when existing model is refuted from the database.  
2. When model is saved for the first time then creating and created event will be fixed.  
3. If a model already exists in database and we update it then updating and updated event will be fixed.  
4. While in both creating/created and updating/updated saving and saved event will be fixed.  
5. Decliting and deleted event will be fixed while deleting a model.  
6. Restoring and restored event will be fixed while restoring a model.  

---

### "Lavavel provides us "3" ways to use these events"

1. Event listener,  
2. Overriding boot function of model itself,  
3. Observer.



# Model life cycle event

## ① Event listener:

```php
protected $dispatchesEvents = [
    'created' => UserCreated::class
];
```

→ Create event named `UserCreated` event.

```php
class UserCreated {
    public $user;
    public function __construct(User $user) {
        $this->user = $user;
    }
}
```

→ Create listener for it now plus an instant materialiser `UserCreatedListener`:

```php
class UserCreatedListener {
    public function handle(UserCreated $event) {
        dd('From UserCreatedListener', $event->user);
    }
}
```

Now register it in `EventServiceProvider`:

```php
protected $listen = [
    UserCreated::class => [
        UserCreatedListener::class
    ]
];
```

Use this approach when you work with real-time projects because such approach allows us to broadcast the data.

```
Here's the converted markdown file based on your image content:

```markdown
# Model life cycle event

## Overriding boot method:

```php
// User.php
public static function boot() {
    parent::boot();
    
    static::created(function ($user) {
        dd('From boot method', $user);
    });
    
    static::updated(function ($user) {
        dd('From boot method', $user);
    });
}
```

---

### Sometimes as your application grows, the need to use observers becomes important.

#### Observer allows you to handle all events in a single class.

---

## Observer:

```bash
php artisan make:observer UserObserver --model=User
```

---

```php
// EventServiceProvider.php
public function boot() {
    parent::boot();
    User::observe(UserObserver::class);
}
```

---

Key points:
- Use `boot()` method in model for simple event handling
- Observers are better for larger applications
- Create observers using Artisan command
- Register observers in EventServiceProvider
```

I've made the following improvements:
1. Fixed "Overliding host method" to "Overriding boot method"
2. Corrected the PHP syntax in code blocks
3. Fixed the Artisan command format
4. Organized the content with clear sections
5. Added a summary of key points
6. Fixed "observer" to "observe" in the EventServiceProvider example
7. Added proper code block formatting

The markdown presents the Laravel model events and observer pattern in a more readable and technically accurate format.