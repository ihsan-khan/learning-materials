## **SQL Concepts**

**1. Understanding the `REPLACE()` Function in SQL**


<details>
	<summary><b>View Detail</b></summary>
<ul>

The `REPLACE()` function is a string manipulation function that substitutes all occurrences of a specified substring with another substring in a given string.

## Basic Syntax:
```sql
REPLACE(original_string, search_string, replacement_string)
```

## In Your Example:
`REPLACE(name, 'a', '')` means:
- Take the value in the `name` column
- Find **all occurrences** of the lowercase letter 'a'
- Replace each 'a' with **an empty string** (effectively removing it)
- Return the modified string

## Key Characteristics:
1. **Case-sensitive**: `REPLACE(name, 'a', '')` will only replace lowercase 'a', not uppercase 'A'
2. **Global replacement**: It replaces ALL occurrences, not just the first one
3. **Returns a new string**: The original string remains unchanged

## Examples:

1. `REPLACE('banana', 'a', '')` → 'bnn'
   - Original: b**a**n**a**n**a**
   - After: b n n

2. `REPLACE('Sarah', 'a', '')` → 'Srh'
   - Original: S**a**r**a**h
   - After: S r h

3. `REPLACE('Adam', 'a', '')` → 'Adm' (only removes lowercase 'a')
   - Original: A d**a** m
   - After: A d m

4. `REPLACE('Alice', 'a', '')` → 'Alice' (no lowercase 'a' to replace)

## Common Uses:
- Removing specific characters from strings
- Standardizing text (e.g., replacing hyphens with spaces)
- Data cleaning (removing unwanted substrings)

Note: In the original query we used `LOWER(name)` first to make it case-insensitive: `REPLACE(LOWER(name), 'a', '')`
</ul>
</details>

**2. What is constrained in SQL and what is purpose of using it**
**3. What is cascadeOnDelete in SQL and what is purpose of using it**
