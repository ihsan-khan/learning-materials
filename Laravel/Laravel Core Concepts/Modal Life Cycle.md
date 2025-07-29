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