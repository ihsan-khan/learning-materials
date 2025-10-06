Here's a comprehensive list of all Laravel Collection methods organized by category:

## **Transformation Methods**
- `map()` - Transform each item
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