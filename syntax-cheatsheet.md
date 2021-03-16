## Data types
Integers `2`

Floats `2.0`

Booleans `true`

Atoms `:ok`

Strings: `"hello"`

**Note: only falsy values in Elixir are `nil` and `false`**

## Operations
Addition: `+`

Subtraction: `-`

Multiplication: `*`

Division
 - float: `/`
 - integer: `div/2`
 - remainder: `rem/2`

Boolean 
- any values: `&&`, `||`, `!`
- first value bool: `and`, `or`, `not`

Comparison
- Equals/Not equals
    - all values: : `==`, `!=`
    - strict floats & ints: `===`, `!==`
- GT/LT: `>`, `<`
- GTE/LTE: `>=`, `<=`

Type comparison: 
```
number < atom < reference < function < port < pid < tuple < map < list < bitstring
```

## String Manipulation
Interpolation: `"String #{interpolation}"`

Concatenation: `"String" <> "concatenation"`

## Lists

Implemented as linked lists. Prepending is faster than appending

Prepending: 
```
value = 1
[value | [2, 3]]
```

Concatenating: 
```
value = 3
[1, 2] ++ value
```

Subtracting (strict comparison):
```
[1,2,3,4.0] -- [3,4]
-> [1,2,4.0]
```

Head
```
hd [3,4,5]
-> 3

[head | tail] = [3,4,5]
head
-> 3
```

Tail
```
tl [3,4,5]
-> [4,5]

[head | tail] = [3,4,5]
tail
-> [4,5]
```

## Tuples
Stored contiguously. Getting length is fast but modifying is expensive.

```
{1,:two, "three"}
```

## Keyword Lists
List of tuples with the following properties:
- keys are atoms
- keys are ordered
- keys do not have to be unique

Most commonly used to pass options to functions

Full syntax
```
[{:foo, "bar"}, {:hello, "world"}]
```

Shorcut syntax
```
[foo: bar, hello: world]
```

## Maps
Main key-value store with following properties:
- keys are unordered
- keys can be any type
- keys can be variables


Creating (mixed keys)
```
map = %{:keyOne => "one", "key2": 2}
```

Creating (:atom keys)
```
map = {keyOne: "one", keyTwo: 2}
```


Accessing (mixed keys)
```
map["key2"]
```

Accessing (atom keys)
```
map.keyTwo
```

Adding new key
```
map = %{hello: "world"}
Map.put(map, :foo, "bar")
```

Updating existing key (creates new map)
```
map = %{hello: "world"}
%{map | hello: "WORLD"}
```

## Enums
Commonly used functions:
```
any, all, chunk_every, chunk_by, each, map_every, map, min, max, filter, reduce, sort, uniq, uniq_by
```

With anonymous function
```
Enum.map([1,2,3], fn number -> number + 3 end)

// Capture operator
Enum.map([1,2,3], &(&1 + 3))
```

With named function
```
defmodule Adding do
  def plus_three(number), do: number + 3
end

// Invoke in anonymous fn
Enum.map([1,2,3], fn number -> Adding.plus_three(number) end)

// Invoke using capture operator
Enum.map([1,2,3], &Adding.plus_three(&1))

// Reference using capture operator
Enum.map([1,2,3], &Adding.plus_three/1)
```

## Pattern Matching

Match operator: `=`
```
list = [1, 2, 3]

[1, 2, 3] = list
-> [1, 2, 3]

[] = list
-> ** (MatchError) no match of right hand side value: [1, 2, 3]
```

Pin operator: `^`

Reference instead of reassign variable in LHS of match operation

```
x = 1

// x is reassigned to 2
// ^x matches on original value of x
{x, ^x} = {2, 1}
-> {2, 1}

// x is now 2
x
-> 2
```


## Control structures

if
```
if String.valid("Hello") do
    "Valid"
else
    "Invalid"
end
```

unless
```
unless is_integer("hello") do
    "Not an Int"
end
```

case
```
case {:ok, "hello"} do
    {:ok, result} -> result
    {:error} -> "Uh oh!"
    _ -> "Catch All"
end
-> "hello"
```

case (with guards)
```
case {1, 2, 3} do
    {1, x, 3} when x > 0 -> "Will match"
    _ -> "Won't match"
 end
```

cond
```
num = 1
cond do
    rem(num, 15) == 0 -> "Fizzbuzz"
    rem(num, 3) == 0 -> "Fizz"
    rem(num, 5) == 0 -> "Buzz"
    true -> num
end
```

with
```
user = %{first: "Sean", last: "Callan"}
with {:ok, first} <- Map.fetch(user, :first),
     {:ok, last} <- Map.fetch(user, :last),
     do: last <> ", " <> first
-> "Callan, Sean"
```

## Functions

### Named functions

Wrap in module
```
defmodule Greeter do
    ...
end
```

full function body
```
def hello(name) do
    "Hello, " <> name
end
```

one line
```
def hello(name), do: "Hello, " <> name
```

with pattern matching
```
def hello(%{name: person_name} = person) do
    IO.puts "Hello, " <> person_name
    IO.inspect(person)
end
```

private: `defp`

with guards
```
def hello(name) when is_binary(name) do
    phrase() <> name
end

defp phrase, do: "Hello, "
```

default arguments
```
def hello(name, language_code \\ "en") do
    phrase(language_code) <> name
end
```

### Anonymous functions

**Full**
```
sum = fn (a, b) -> a + b end
```

**Shorthand**
```
sum = &(&1 + &2)
```

**With pattern matching**

Define function
```
handle_result = fn
    {:ok, result} -> IO.puts "Will handle"
    {:ok, _} -> IO.puts "Previous will handle."
    {:error} -> IO.puts "Error!"
end
```

Call function
```
handle_result.({:ok, some_result})
-> "Handling result..."

handle_result.({:error})
-> "Error!"
```

## Pipe operator
```
other_function() |> new_function() |> baz() |> bar() |> foo()
```




