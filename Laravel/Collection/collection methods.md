A comprehensive list of all Laravel Collection methods organized by category:

## **Transformation Methods**
- `map()` - Transform each item
<details>
	<summary><b>View Detail</b></summary>
<ul>
# **Laravel Collection `map()` Method - Detailed Explanation**

## **What is `map()`?**
The `map()` method iterates over each item in a collection, applies a callback function to it, and returns a new collection with the modified items.

## **Basic Syntax**
```php
$newCollection = $originalCollection->map(callback $callback);
```

## **1. Simple Value Transformation**

### **Example: Square Numbers**
```php
$numbers = collect([1, 2, 3, 4, 5]);

$squared = $numbers->map(function ($number) {
    return $number * $number;
});

// Result: [1, 4, 9, 16, 25]
```

### **Example: Convert to Uppercase**
```php
$names = collect(['john', 'jane', 'doe']);

$uppercased = $names->map(function ($name) {
    return strtoupper($name);
});

// Result: ['JOHN', 'JANE', 'DOE']
```

## **2. Working with Arrays/Objects**

### **Example: Transform User Data**
```php
$users = collect([
    ['name' => 'John Doe', 'age' => 25],
    ['name' => 'Jane Smith', 'age' => 30],
    ['name' => 'Bob Johnson', 'age' => 35]
]);

$formattedUsers = $users->map(function ($user) {
    return [
        'full_name' => $user['name'],
        'years_old' => $user['age'],
        'is_adult' => $user['age'] >= 18
    ];
});

// Result:
// [
//     ['full_name' => 'John Doe', 'years_old' => 25, 'is_adult' => true],
//     ['full_name' => 'Jane Smith', 'years_old' => 30, 'is_adult' => true],
//     ['full_name' => 'Bob Johnson', 'years_old' => 35, 'is_adult' => true]
// ]
```

## **3. Using with Index/Key Parameter**

### **Example: Include Index in Transformation**
```php
$fruits = collect(['apple', 'banana', 'orange']);

$indexedFruits = $fruits->map(function ($fruit, $index) {
    return [
        'id' => $index + 1,
        'name' => $fruit,
        'position' => $index
    ];
});

// Result:
// [
//     ['id' => 1, 'name' => 'apple', 'position' => 0],
//     ['id' => 2, 'name' => 'banana', 'position' => 1],
//     ['id' => 3, 'name' => 'orange', 'position' => 2]
// ]
```

## **4. Real-World Education Example (Your Code)**

```php
$educationData = collect($request->education_array)->map(function ($education, $index) use ($request) {
    return [
        'user_profile_id' => $request->user_profile_id,
        'degree_name' => $education['degree_name'],
        'start_date' => $education['start_date'],
        'end_date' => $education['end_date'],
        'institute' => $education['institute'],
        'display_order' => $index, // Added index as display order
        'created_at' => now(),
        'updated_at' => now(),
    ];
})->toArray();
```

## **5. Advanced Transformations**

### **Example: Calculate Derived Values**
```php
$products = collect([
    ['name' => 'Laptop', 'price' => 1000, 'quantity' => 2],
    ['name' => 'Mouse', 'price' => 50, 'quantity' => 5],
    ['name' => 'Keyboard', 'price' => 80, 'quantity' => 3]
]);

$productSummary = $products->map(function ($product) {
    $totalValue = $product['price'] * $product['quantity'];
    $tax = $totalValue * 0.15; // 15% tax
    
    return [
        'product_name' => $product['name'],
        'unit_price' => $product['price'],
        'total_value' => $totalValue,
        'tax_amount' => $tax,
        'final_price' => $totalValue + $tax
    ];
});

// Result includes calculated fields
```

### **Example: Conditional Transformations**
```php
$students = collect([
    ['name' => 'Alice', 'score' => 85],
    ['name' => 'Bob', 'score' => 45],
    ['name' => 'Charlie', 'score' => 92],
    ['name' => 'Diana', 'score' => 38]
]);

$gradedStudents = $students->map(function ($student) {
    $grade = match(true) {
        $student['score'] >= 90 => 'A',
        $student['score'] >= 80 => 'B',
        $student['score'] >= 70 => 'C',
        $student['score'] >= 60 => 'D',
        default => 'F'
    };
    
    $status = $student['score'] >= 60 ? 'Pass' : 'Fail';
    
    return [
        'student_name' => $student['name'],
        'score' => $student['score'],
        'grade' => $grade,
        'status' => $status
    ];
});
```

## **6. Using External Variables with `use()`**

### **Example: Apply Discount Rate**
```php
$discountRate = 0.10; // 10% discount

$products = collect([
    ['name' => 'Product A', 'price' => 100],
    ['name' => 'Product B', 'price' => 200],
    ['name' => 'Product C', 'price' => 150]
]);

$discountedProducts = $products->map(function ($product) use ($discountRate) {
    $discountedPrice = $product['price'] * (1 - $discountRate);
    
    return [
        'name' => $product['name'],
        'original_price' => $product['price'],
        'discounted_price' => round($discountedPrice, 2),
        'savings' => $product['price'] - $discountedPrice
    ];
});
```

## **7. Method Chaining with `map()`**

### **Example: Multiple Transformations**
```php
$data = collect([10, 20, 30, 40, 50, 60, 70, 80, 90, 100]);

$processedData = $data->filter(function ($number) {
    return $number > 30; // Keep only numbers greater than 30
})->map(function ($number) {
    return $number * 2; // Double each number
})->reject(function ($number) {
    return $number > 150; // Remove numbers greater than 150
})->values(); // Reset keys

// Result: [80, 100, 120, 140]
```

## **8. Arrow Functions (PHP 7.4+)**

### **Example: Concise Syntax**
```php
$numbers = collect([1, 2, 3, 4, 5]);

// Traditional function
$squaredTraditional = $numbers->map(function ($number) {
    return $number * $number;
});

// Arrow function (shorter)
$squaredArrow = $numbers->map(fn($number) => $number * $number);

// Both produce same result: [1, 4, 9, 16, 25]
```

## **9. Important Characteristics of `map()`**

### **Immutable Operation**
```php
$original = collect([1, 2, 3]);
$mapped = $original->map(fn($n) => $n * 2);

$original->all(); // [1, 2, 3] - unchanged!
$mapped->all();   // [2, 4, 6] - new collection
```

### **Preserves Keys**
```php
$keyedCollection = collect([
    'a' => 1,
    'b' => 2, 
    'c' => 3
]);

$result = $keyedCollection->map(fn($n) => $n * 10);

// Result: ['a' => 10, 'b' => 20, 'c' => 30] - keys preserved!
```

## **10. Common Use Cases**

### **API Response Transformation**
```php
$apiResponse = collect($externalApiData)->map(function ($item) {
    return [
        'id' => $item['external_id'],
        'title' => $item['name'],
        'description' => $item['details']['summary'],
        'metadata' => [
            'created_at' => Carbon::parse($item['created']),
            'updated_at' => Carbon::parse($item['modified'])
        ]
    ];
});
```

### **Database Result Formatting**
```php
$users = User::all()->map(function ($user) {
    return [
        'user_id' => $user->id,
        'full_name' => $user->first_name . ' ' . $user->last_name,
        'email' => $user->email,
        'profile_url' => url('/users/' . $user->id),
        'registered_at' => $user->created_at->format('M j, Y')
    ];
});
```

## **Key Takeaways**

1. **Returns new collection** - original remains unchanged
2. **Preserves keys** - unless you explicitly reset them
3. **Accepts callback** - that receives value and optional key
4. **Chainable** - can be combined with other collection methods
5. **Versatile** - can transform data in any way you need

The `map()` method is one of the most powerful and frequently used collection methods in Laravel, essential for data transformation tasks!
</ul>
</details>

- `transform()` - Transform the collection itself
- `flatMap()` - Transform then flatten
- `pluck()` - Extract values for a given key
- `keys()` - Get all keys
- `values()` - Reset keys
- `zip()` - Merge with given arrays
- `collapse()` - Collapse array of arrays into single array
- `flatten()` - Flatten multi-dimensional collection
- `reverse()` - Reverse items order

## **Filtering Methods**
- `filter()` - Filter by callback
- `where()` - Filter by key/value
- `whereIn()` - Filter where key in array
- `whereNotIn()` - Filter where key not in array
- `whereBetween()` - Filter where key between values
- `whereNotBetween()` - Filter where key not between values
- `whereNull()` - Filter where key is null
- `whereNotNull()` - Filter where key is not null
- `first()` - Get first element
- `last()` - Get last element
- `firstWhere()` - Get first element where condition
- `reject()` - Remove items using callback

## **Sorting Methods**
- `sort()` - Sort collection
- `sortBy()` - Sort by key
- `sortByDesc()` - Sort by key descending
- `sortKeys()` - Sort by keys
- `sortKeysDesc()` - Sort by keys descending
- `reverse()` - Reverse order

## **Aggregate Methods**
- `count()` - Count items
- `sum()` - Sum values
- `avg()` - Average of values
- `average()` - Alias of avg()
- `min()` - Minimum value
- `max()` - Maximum value
- `median()` - Median value
- `mode()` - Mode value

## **Grouping Methods**
- `groupBy()` - Group by key
- `keyBy()` - Key by attribute
- `partition()` - Separate by condition

## **Search Methods**
- `contains()` - Check if contains value
- `containsStrict()` - Strict contains check
- `has()` - Check if has key
- `search()` - Search for value
- `find()` - Find by key

## **Mathematical Methods**
- `min()` - Minimum value
- `max()` - Maximum value
- `sum()` - Sum values
- `avg()` - Average
- `median()` - Median
- `mode()` - Mode

## **Chunking & Splitting**
- `chunk()` - Split into chunks
- `split()` - Split into groups
- `chunkWhile()` - Chunk while condition
- `nth()` - Get every nth element
- `sliding()` - Get sliding window of items

## **Set Operations**
- `diff()` - Difference between collections
- `diffAssoc()` - Difference with index check
- `diffKeys()` - Difference by keys
- `intersect()` - Intersection of collections
- `intersectByKeys()` - Intersect by keys
- `union()` - Union with array
- `unique()` - Unique values
- `uniqueStrict()` - Unique with strict comparison

## **Conditional Methods**
- `when()` - Apply when true
- `whenEmpty()` - Apply when empty
- `whenNotEmpty()` - Apply when not empty
- `unless()` - Apply unless true
- `unlessEmpty()` - Apply unless empty
- `unlessNotEmpty()` - Apply unless not empty

## **Joining & Combining**
- `merge()` - Merge with array
- `mergeRecursive()` - Merge recursively
- `combine()` - Combine with keys
- `concat()` - Concatenate values
- `join()` - Join values with string
- `crossJoin()` - Cross join with arrays

## **Reduction Methods**
- `reduce()` - Reduce to single value
- `reduceSpread()` - Reduce with multiple returns

## **Boolean Methods**
- `isEmpty()` - Check if empty
- `isNotEmpty()` - Check if not empty
- `contains()` - Check if contains
- `containsOneItem()` - Check if contains one item
- `doesntContain()` - Check if doesn't contain

## **Conversion Methods**
- `toArray()` - Convert to array
- `toJson()` - Convert to JSON
- `jsonSerialize()` - Prepare for JSON
- `all()` - Get underlying array
- `get()` - Get item by key

## **Execution Methods**
- `each()` - Iterate over items
- `eachSpread()` - Iterate with spread
- `tap()` - Tap into collection
- `pipe()` - Pipe through callback

## **Cache Methods**
- `remember()` - Remember result
- `rememberForever()` - Remember forever

## **Higher Order Messages**
- `->each->method()` - Call method on each item
- `->filter->property` - Filter by property
- `->map->property` - Map to property

## **Newer Methods (Laravel 8+)**
- `sole()` - Get only item or throw
- `firstOrFail()` - First or throw exception
- `value()` - Get first value
- `collect()` - Create new collection
- `undot()` - Expand dotted keys
- `mapInto()` - Map into class
- `mapToDictionary()` - Map to dictionary
- `mapToGroups()` - Map to groups

## **Practical Examples of Common Methods**

### **Pluck - Extract Values**
```php
$users = collect([
    ['id' => 1, 'name' => 'John'],
    ['id' => 2, 'name' => 'Jane']
]);

$names = $users->pluck('name'); // ['John', 'Jane']
```

### **GroupBy - Group Items**
```php
$education = collect([
    ['degree' => 'BS', 'year' => 2020],
    ['degree' => 'MS', 'year' => 2020],
    ['degree' => 'BS', 'year' => 2021]
]);

$grouped = $education->groupBy('year');
```

### **Where - Filter Items**
```php
$users = collect([
    ['name' => 'John', 'active' => true],
    ['name' => 'Jane', 'active' => false]
]);

$activeUsers = $users->where('active', true);
```

### **Chunk - Split into Groups**
```php
$numbers = collect([1, 2, 3, 4, 5, 6]);
$chunks = $numbers->chunk(2); // [[1,2], [3,4], [5,6]]
```