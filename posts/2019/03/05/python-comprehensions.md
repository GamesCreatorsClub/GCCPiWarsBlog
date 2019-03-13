Title: Python Comprehensions
Tags: Home, Blog, piwars, python
Date: 2019-03-05 20:35
Author: Daniel Sendula
Background-image: /2019/03/opengraph-icon-200x200.png

# Python Comprehensions

Where were many posts how with pictures. Let's have one with some code.

Over last couple of months I've got impression that some people do not regard Python as good, modern language (well it has some of the history to make some people stop and think). As any language it has its good and bad sides along with where it can be used as a really good choice and where maybe not.

Python is quite a high level programming language which had its object orientation added relatively late and some of it reflect in somewhat clumsy syntax. But, there are some other sides of Python that put is at par with other popular languages like Java/C#, Ruby, JavaScript and such. Some of it's syntax sugar is making it even better... And that's what we'll look into here: list and dictionary comprehensions.

<!-- TEASER_END -->

## The problem

Our code is separated in many small processes which acquire data from sensors, inputs and such, process them and send them by MQTT messages to high level controllers that make decisions and send them to lower level controllers that know how to command wheels, and then from there to wheel controllers that steer and drive them. For simplicity originally we adopted sending messages in plain ASCII - human readable form so debug can be really easy. For instance shell command:

`mosquitto_sub -v -t wheels/#`

would show required positions and speeds of wheels,

`mosquitto_sub -v -t move/#`

would show what was required of rover to do - rotate, drive in straight line at angle or steer around some point at some distance form the rover.

Distance sensors do provide similar messages that look like this:

`0:1201 45:872 90:563 135:797 180:1622 225:433 270:319 315:478`

## Solution

To transform it from string like that to a dictionary we can use simple code like this:

```python

distances = {}
for entry in message.split(" "):
    split = entry.split(":")
    distances[split[0]] = split[1]
```

But with list comprehensions we can do better:

```python
distances = {int(k):float(v) for k,v in [entry.split(":") for entry in message.split(" ")]}
```

Let's see what it really does:

```python
message.split(" ")
```

is making a list of strings that are separated by given separator (an empty string here `" "`) and result is:

`['0:1201', '45:872', '90:563', '135:797', '180:1622', '225:433', '270:319', '315:478']`

Next is to take list of strings that are separated by `':'` and make a list of two element lists:

```python
[entry.split(":") for entry in message.split(" ")]
```

Result of it is:

`[['0', '1201'], ['45', '872'], ['90', '563'], ['135', '797'], ['180', '1622'], ['225', '433'], ['270', '319'], ['315', '478']]`

That's called a list comprehension. And in there we can filter elements and/or transform them on the fly. For instance, if we have a list of strings that represent numbers like this:

`['1', '2', '3', '4', '5']`

we can transform it to list of numbers:

```python
[int(n) for n in ['1', '2', '3', '4', '5']]
```

where result is:

`[1, 2, 3, 4, 5]`

Also, we can transform list of numbers so we exclude all greater than 2:

```python
[n for n in [1, 2, 3, 4, 5] if n > 2]
```

where result is:

`[3, 4, 5]`

Now back to our problem. We've produced list of lists of two elements. Now we can use those two elements and produce a dictionary using first element as key and second as value:

```python
{int(k):float(v) for k,v in [entry.split(":") for entry in message.split(" ")]}
```

Also, just before using key we will transform it to `int` and value to `float`.

Result is:

`{0: 1201.0, 45: 872.0, 90: 563.0, 135: 797.0, 180: 1622.0, 225: 433.0, 270: 319.0, 315: 478.0}`

But we had another problem to deal with: sometimes we have data passed to us at various parts of the code as just a list without keys given - where keys are understood. For instance logging: we have `record` containing values that were previously logged (something like `[1550348686.8102736,b'fr',b'SK',1.0,32,0.0,0.0,0.03261876106262207,0.0,0.0,0.0,0.0]`) and list of fields that were used to create that record. Each field is an python class that has `toString(value)` and `fromString(value)`methods. First is to produce csv file and second to create value from csv's column.

So, to create CSV file (which we can use later on for analysing what went wrong) we would like to create a string out of record and back to create a record back of string. But our data and data definitions are in separate arrays. Ordinary solution people would normally go with (and including me before I got fascinated with comprehensions) would go something like this:

```python

result = {}
for i in range(len(logger_def.fields)):
    result[field.fields[i].name] = field.fields[i].fromString(record[i])
```

But now with comprehensions we can do better. Also, in order to put two arrays together we can use method 'zip' which alternates elements from one and another array. For instance:

```python
zip([1, 2, 3, 4], ['a', 'b', 'c', 'd'])
```

would give:

`[(1, 'a'), (2, 'b'), (3, 'c'), (4, 'd')]`

(not really - would we need to 'convert' iterator to a list with something like this
```python
[e for e in zip([1, 2, 3, 4], ['a', 'b', 'c', 'd'])]
```
 but final result is the same)

Now we can combine these two together:

```python
result = {f.name: f.fromString(v) for f, v in zip(logger_def.fields, line.split(","))}
```

(try simple example from above:
```python
{k: v for k, v in zip([1, 2, 3, 4], ['a', 'b', 'c', 'd'])}
```
)

## Conclusion

Those are just one aspect of Python that make it interesting and fun to work with. There are much more to the language...