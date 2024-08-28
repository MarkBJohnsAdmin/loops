# Loops

## While Loops

We've already gone over conditionals, or the if, elif, and else blocks that check for true boolean values before executing code, but if you have code that needs to increment in some way, you can automate that conditional checking for each step along the way. Put a lot more simply, you can start with a base state, check if the base state meets certain criteria, and then change the base state. You'll then apply that same check on the new state, and perform that same change if it meets the criteria. This is a called a `while loop`, and the simplest way to demonstrate this is with a counter function.

```py
def count_to_ten(start=1):
    while start < 10:
        print(start)
        start += 1
    print(10)
    return
```

The `start=1` parameter means that "start" is your argument, but if no argument is included, the value of start will be 1 by default. So when this function is called, it starts with "while 1 < 10". Given that this statement is true, the code inside the while block is executed. The code is to print the current value of start, and then add 1 ("start += 1" is a shortcut for "start = start + 1"). The first time, the value of start is 1, so we print "1", then rewrite the value of start to 2. Because we're in a loop, instead of moving on to the rest of the code, we actually go back to the beginning of the loop and check the condition again. Now we're checking "while 2 < 10", which is still true, so the loop runs again. This will continue until the value of start is no longer less than 10.

```py
count_to_ten()

# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# Here, we get to 10 < 10, which is false, so the loop ends
# 10 - but we have it manually print "10" outside of the loop to catch this.
```

We do have a few options we can use to avoid setting data outside of our loop:

```py
def count_to_ten(start=1):
    while start <= 10:
        print(start)
        start += 1
    return
    
# This works, because 10 <= 10 is true, but if fails to count to ten if you use a float instead of an integer:

count_to_ten(1.1)

# 1.1
# 2.1
# 3.1
# 4.1
# 5.1
# 6.1
# 7.1
# 8.1
# 9.1

# 10.1 <= 10 is False, so we never actually reach 10
```

```py
def count_to_ten():
    start = 1
    while start <= 10:
        print(start)
        start += 1
    return
    
count_to_ten()

# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# 10

# Always works, but completely removes any user input
```

The initial version that manually prints 10 will work for any numerical starting point less than 10, and end on exactly 10:

```py
count_to_ten(0.7)

# 0.7
# 1.7
# 2.7
# ...
# 9.7
# 10
```

So it's up to you and what your specific code base needs which option makes the most sense.

## Important - Permanent Loop Error

It's easy to make a while loop that goes on forever if you aren't careful. The function won't actually go on forever, but only because your computer won't be able to handle it, and not for lack of trying.

```py
def melt_your_cpu(start=1):
    while start != 11:
        print(start)
        start += 1
    return
```

This looks like it makes sense, and provided you have the correct input, it will.

```py
def melt_your_cpu(start=1):
    while start != 11:
        print(start)
        start += 1
    return

melt_your_cpu()

# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# 10
```

However, you should always assume that people won't know how to use your code, and them not knowing how to use it should be as harmless as possible. This function assumes the user will always start with an integer that's less than 11, but we have no way of enforcing that. So if the user were to pass in something like 12, or 1.2, we have a problem:

```py
melt_your_cpu(12)

# 12
# 13
# 14
# 15
# 16
# ...

# At no point will start ever equal 11, as it started greater than 11 and is only going up. This will go on for infinity.
```

```py
melt_your_cpu(1.2)

# 1.2
# 2.2
# 3.2
# ...
# 9.2
# 10.2
# 11.2
# 12.2

# This starts off pretty close, but close doesn't count in programming. It gets to 11.2, but 11.2 isn't 11, so the loop continues, and again will never actually get to 11 to stop the loop.
```

## For Loops

A more common, and less risky type of loop is the `for loop`. Rather than making consectutive boolean checks until one of them is false, a for loop iterates through a pre-existing number of values. Meaning, for each item in an iterable object, perform a function. The easiest way to demonstrate this is by making the same counter function with a for loop instead of a while loop:

```py
def count_to_ten():
    count = range(1, 11)
    for num in count:
        print(num)
    return
    
count_to_ten()

# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# 10
```

Our `range` function created a list of numbers, `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`, and our for loop went down the list, calling the `print` function on each of them in order. There's no boolean value, there's no end condition, and there's no risk of an infinite, cpu-melting loop. There's just a preset list of values, and a function call for each of them. It's useful both as a simple way to execute a command a specified amount of times, or to run a useful formatting function for every item in a collection.

```py
def repeat_yourself(count):
    for num in range(0, count):
        print("Hello")

repeat_yourself(3)

# Hello
# Hello
# Hello

def introduce_yourself(people):
    for person in people:
        print(f"Hello, my name is {person}")
        
people = ["Mark", "Steven", "Sarah", "Nathan"]

introduce_yourself(people)

# Hello, my name is Mark
# Hello, my name is Steven
# Hello, my name is Sarah
# Hello, my name is Nathan
```

You can pair this with conditionals to filter out the types of data you want as well:

```py
def no_vowels(string):
    vowels = ['a', 'e', 'i', 'o', 'u']
    message = ''
    for char in string:
        if char not in vowels:
            message += char
    return message

print(no_vowels("Hello world"))

# Hll wrld

def only_odds(nums):
    odds = []
    for num in nums:
        if num % 2 != 0:
            odds.append(num)
    return odds

print(only_odds([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))

# [1, 3, 5, 7, 9]
```

Python actually has a `filter' method built in, but behind the scenes, this is all it's doing: running a for loop on your iterable and checking for a condition.

```py
def no_vowels(string):
    vowels = ['a', 'e', 'i', 'o', 'u']
    message = ''
    for char in string:
        if char not in vowels:
            message += char
    return message # returns a formatted string
    
def isnt_vowel(char):
    vowels = ['a', 'e', 'i', 'o', 'u']
    return char not in vowels # returns a boolean
    
string1 = no_vowels("Hello world")

string2 = ''.join(filter(isnt_vowel, "Hello world"))

# Both return "Hll wrld"

def only_odds(nums):
    odds = []
    for num in nums:
        if num % 2 != 0:
            odds.append(num)
    return odds
    
def is_odd(num):
    return num % 2 != 0
    
list1 = only_odds([1, 2, 3, 4, 5])

list2 = list(filter(is_odd, [1, 2, 3, 4, 5]))

# Both return [1, 3, 5]
```

The filter function is built-in so it's usually better to use it over making a custom filtration function, but it's good to know exactly what it's doing behind the curtain.

## List Comprehension

A special kind of for loop you can use in Python looks a little janky if you're not used to it, but it's incredibly useful. In a typical for loop, you're performing an action for each value, but each of those values are disconnected. We saw this in the for loop version of count_to_ten, where there were ten independent print() function calls. You can connect the data, which you can see in both only_odds and no_vowels, but we create an empty list / string at the beginning of the function and add each true value into that new iterable.

If you're starting out with an iterable and you want your result to be another iterable, you can use a shortcut called `list comprehension`. The format for list comprehensions is:

```py
[expression for item in iterable]
```

The `for item in iterable` should look familiar, as that's what all of our for loops have been using. The difference is that the function you're calling is no longer nested within the scope of your for loop, but at the beginning of the for loop. The for loop is also wrapped in brackets, because the end result will be a new, reformatted list.

```py
nums = [1, 2, 3, 4, 5]

def trad_dub(nums):
    '''Traditional for loop'''
    doubled = []
    for num in nums:
        doubled.append(num * 2)
    return doubled
    
def lst_comp_dub(nums):
    '''List comprehension'''
    return [num * 2 for num in nums]
    
trad = trad_dub(nums)

lst_comp = lst_comp_dub(nums)

# Both will return [2, 4, 6, 8, 10]
```

## Enumerate

Finally, if you want to keep track of where you are in the loop, you can use the `enumerate` function to divide your iteratable items into the value and the index. To use this, be sure to declare both a name for the index and a name for the value:

```py
pokemon = ["bulbasaur", "ivysaur", "venusaur"]

for idx, pkmn in pokemon:
    print(f"Dex Number {idx + 1}: {pkmn}")
    
# Dex Number 1: bulbasaur
# Dex Number 2: ivysaur
# Dex Number 3: venusaur 

#Note that Python is zero-indexed, which can be awkward for non-programmers and make make data actually wrong if you're referencing external data, like a Pokedex. Adding 1 to the value just helps readability.
```
