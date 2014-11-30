---
layout: post
title: PHP Large Size Array
---

We can use directly in_array method to check existing. For example:

```
$some_values = array(1, 2, 3, 4, 5);
var_dump(in_array(1, $some_values));
// bool(true)
```

The result is clear. `$some_values` array has "1" element as a value. Sometimes we must check all the array to compare each other. For example, we have a number list in an array and we try to check singularity of all elements.

First Example:

```
$some_values = array(1, 2, 3, 3, 4, 5);
$temporary_values = array();
for ($i = 0; $i < count($some_values); $i++) {
    $found = false;
    for ($j = 0; $j < count($temporary_values); $j++) {
        if ($temporary_values[$j] == $some_values[$i]) {
            $found = true;
            break;
        }
    }
    if (!$found) {
        // Checking or processing some other things
        $temporary_values[] = $some_values[i];
    }
}
print_r($temporary_values); // All elements of array is singular
```

Second Example:

```
$some_values = array(1, 2, 3, 4, 5);
$temporary_values = array();
for ($i = 0; $i < count($some_values); $i++) {
    if (!in_array($some_values[$i], $temporary_values)) {
        // Checking or processing some other things
        $temporary_values[] = $some_values[$i];
    }
}
print_r($temporary_values); // All elements of array is singular
```

In two example, run time is the same and they are O(n2) complexity in worst case. You can check [in_array](https://github.com/php/php-src/blob/master/ext/standard/array.c#L1290) method to understand why they are the same. `in_array` method use [php_search_array](https://github.com/php/php-src/blob/master/ext/standard/array.c#L1227) method directly. And they have a loop to move on array. 

If we have some small array, in_array function save our time. Do we have large size array? If we have an array and it has 10,000+ elements. In this time, php return us some errors like that:

```
Fatal error: Maximum execution time of 30 seconds exceeded in .... on line 4
```
Because, our application execution time is more than our php configs. There are two solution for that. First one is change your `max_execution_time` from your php.ini file. Second one is change your application. 

I will try to explain how we can change our application for this spesific problem. Firstly, we will get rid of inner loop. 

```
$some_values = array("a1", "a2", "a3", "a4", "a5", ..., "a999999", "a1000000");
$temporary_array = array();
for ($i = 0; $i < count($some_values); $i++) {
    $temporary_array[$some_values[$i]] = $i;
}
$temporary_values = array();
foreach ($temporary_array as $key => $value) {
    $temporary_values[] = $key;
}
print_r($temporary_values); // All elements of array is singular
```

To minimize out code, we can use some php standart function. Firstly, we use `array_flip` to create `temporary_array`. 

```
$some_values = array("a1", "a2", "a3", "a4", "a5", ..., "a999999", "a1000000");
$temporary_array = array_flip($some_values);
$temporary_values = array();
foreach ($temporary_array as $key => $value) {
    $temporary_values[] = $key;
}
print_r($temporary_values); // All elements of array is singular
```

Try to use `array_keys` to get keys of `temporary_array`. 

```
$some_values = array("a1", "a2", "a3", "a4", "a5", ..., "a999999", "a1000000");
$temporary_array = array_flip($some_values);
$temporary_values = array_keys($temporary_array);
print_r($temporary_values); // All elements of array is singular
```

Now remove some unused variables from our code. 

```
$some_values = array("a1", "a2", "a3", "a4", "a5", ..., "a999999", "a1000000");
$temporary_values = array_keys(array_flip($some_values));
print_r($temporary_values);
```

Thank you.


#### Some Links

 * [in_array function source](https://github.com/php/php-src/blob/master/ext/standard/array.c#L1290)
 * [php_search_array function source](https://github.com/php/php-src/blob/master/ext/standard/array.c#L1227)
 * [array_flip function source](https://github.com/php/php-src/blob/master/ext/standard/array.c#L2919)
 * [array_keys function source](https://github.com/php/php-src/blob/master/ext/standard/array.c#L2603)