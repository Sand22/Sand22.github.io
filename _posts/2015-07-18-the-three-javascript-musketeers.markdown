---
layout: post
title:  "The three JavaScript musketeers"
date:   2015-06-19
lang: en
image: /assets/article_images/level-up/musketeers.png
---


Every once in a while there is a need for collections to be transformed somehow.
Perhaps the REST service could respond with redundant data and we need to filter
it out before displaying. Or maybe we want to calculate something based on object
properties and thereafter extending the object with the result of our computation.
Either way, I believe you would find yourself calm if knew about the existence of
three amazingly helpful functions, that is: map, filter and reduce.

The majority of data transformations (and all of the examples provided in this
post) should be easily achieved in the imperative way. If you feel more familiar
with simple statements as for loops, then I highly encourage you to provide
alternatives for the following code snippets. Then you can easily assess which
style in which case is more suitable for you.

Honestly, at my daily basis I rarely use for loops anymore. Most of the time
I find delivered map-, filter-, reduce-based solutions more readable and far
less time consuming. And for the other use cases there are always some lodash
utilities, which can reduce number of keystrokes A LOT.

![Disclaimer: this is definitely not me a few years ago](/assets/article_images/level-up/bear.jpeg)

## Give me the map and I’ll show you the way

The map function is pretty simple - it takes a callback function which is executed
on each element of array that is traversed and returns a new array. The callback
can take an array element, index of this element and the array itself as input arguments.
Let's look at an example:

```javascript
 1  var people = [{
 2    id: 1,
 3    name: 'John',
 4    dateOfBirth: 204832231000
 5  }, {
 6    id: 2,
 7    name: 'Adam',
 8    dateOfBirth: 509027652000
 9  }, {
10     id: 3,
11     name: 'Robert',
12     dateOfBirth: -1203727670000
13   }];
14
15   people = people.map(person => person.name);
```

As you can see, we have a list of people defined and our desired goal is to
get only their names as a flat list of strings. To achieve that we simply
call map on the list and give an arrow function as a callback (line 15). The arrow
function (available in ES 6) returns *name* field value, so that in the end we will get
the list of names.

Considering we have the people from the previous example, we may wish to
know what age they are.

```javascript
 1  people = people.map((person) => {
 2    var yearInMilliseconds = 1000 * 60 * 60 * 24 * 365,
 3      ageInMilliseconds = new Date() - person.dateOfBirth;
 4
 5    return {
 6      id: person.id,
 7      name: person.name,
 8      age: Math.floor(ageInMilliseconds / yearInMilliseconds)
 9    };
10   });
```

Again map is very helpful in this case. Notice that in line 5 we returned
object with specific fields. It seems that we can skip *dateOfBirth*, which is no
longer needed after calculating age. But what if we wanted to preserve every field
in the object? Well, we could explicitly return all the fields, but that's just silly.
And remember, the callback cannot modify the array being traversed.
In fact, there is a common function *extend* which we're going to use. So if we want
for instance add only field *isYoung* to our object we could write something like this:

```javascript
 1  people.map((person) => {
 2    var yearInMilliseconds = 1000 * 60 * 60 * 24 * 365,
 3      ageInMilliseconds = new Date() - person.dateOfBirth;
 4
 5    return extend(person, {
 6      isYoung: Math.floor(ageInMilliseconds / yearInMilliseconds) < 50
 7    });
 8  });
```

It is very likely you don't have *extend* function in your toolset. Some utility
libraries such as the mentioned lodash have their own implementation of *extend*.
But for the sake of simplicity we won't use them, but just write our own little guy.

```javascript
 1  function extend(source, destination) {
 2    for (key in destination) {
 3      if (destination.hasOwnProperty(key)) {
 4        source[key] = destination[key];
 5      }
 6    }
 7
 8    return source;
 9  }
```

The extend takes 2 arguments: source and destination. The idea behind this is to
fill the source object with fields from destination object. It doesn't matter if
the field already exists. In that case it would be just replaced with the value from
destination object.

It's not that hard, isn't it? Most of the time there is no need to reinvent the wheel,
so it would be the best to reuse existing utility functions. But sometimes it's
not worth it to get the whole library if you want only one short functionality.

## The quite obvious filter

The filter function just filters elements on the list. That's it, end of story.

The name should be straightforward and the usage is also not complicated. The truth is,
many examples I've seen on the Internet are indeed very similar. But don't underestimate
this little guy. Behind the scenes it's very powerful and due to the repeatable need
of filtering data on the client, also extremely useful.

Back into code. Assume we've done mapping people and eventually we have their
age calculated.

```javascript
 1  var people = [{
 2    id: 1,
 3    name: 'John',
 4    age: 39
 5  }, {
 6    id: 2,
 7    name: 'Adam',
 8    age: 29
 9  }, {
10     id: 3,
11     name: 'Jack',
12     age: 84
13   }];
```

Now, let's imagine we've created some custom filters for our users to easily narrow down the list
of people by chosen criteria. So, for instance we can define the age range or get
folks that name starts with given letter and so on.

A few lines of code and we're good to go:

```javascript
 1  var ageFrom = 28,
 2  	ageTo = 40,
 3  	letter = 'J';
 4  people.filter(person => person.age < ageFrom && person.age > ageTo);
 5  people.filter(person => person.name.slice(0) === letter);
```

We put 2 conditions in lines 4 and 5. They determine whether the element
that is currently passed to our callback should or shouldn't be inserted
in a newly created list. The filter function similarly to the map returns
new array, so in this case the list of people is untouched.

## Reducing complexity by reducing data

The final candidate for the golden cup is the *reduce* function. Unfortunately,
it seems to be much more complex than its colleagues, thus needs more than just
a brief explanation.

When I've met *reduce* for the first time of my life, I was little confused.
It wasn't easy for me to remember the syntax and frankly speaking I didn't buy
its usage. Took me several weeks to really appreciate the amount of time reduced (pun intented)
when finally *got it*.

What's all the fuss about it?

First of all, reduce, as you might expect, takes a callback function which
therefore takes 4 parameters: previous and current element, index of the current
element, the array reduced was called upon. You can also provide initial value
for the reduce function, but it's fully optional.

Secondly, it iterates over the array and set the previous and current element
based on the result returned by the previous call. So, if you have for instance
array of 3 elements and execute reduce on it that takes a callback and simply returns
current element, you end up with something like this:

> [1,2,3] -> reduce

> 1 iteration: prev = 1, curr = 2 if optional is not set

> 2 iteration: prev = 2, curr = 3 if optional is not set

> returns 3

If optional argument is provided, for example *0*, then the 1st iteration
is slightly different and results in *prev* being *0* and *curr* being *1*. The rest is
just shifted resulting in *3* as well.

The important point to note is that reduce results in a single value that can be
just anything: number, string or even another array.

Ok, now having the basics grasped, we can dive deep in some useful examples.

We still didn't get enough of our list of people. It came to my mind, that we could
have made use of their average age. So let's try to fix that by using reduce:

```javascript
 1  var averageAge = people.reduce((prev, curr) => {
 2    return prev + curr.age;
 3  }, 0) / people.length;
```

That was neat. We iterated over the list of people and on each called returned
the sum of previous person age and current person age. We introduced initial value
of 0. If we didn't and sticked to the *prev.age + curr.age* we would end up with
the *NaN* result, because in the second iteration the resulted number is not an object
with an age property.

Now, imagine our list of people keeps growing. The amount of people is too damn high
to easily find the one we're looking for by just looking at the list. So, let's create
a little helper function *findById* which internally will make use of reduce function.

```javascript
 1  function findById(list, id) {
 2    return list.reduce((prev, curr, index, arr) => {
 3      if (prev.id === id) {
 4        return prev;
 5      }
 6
 7      if (arr.length === index + 1) {
 8        return undefined;
 9      }
10
11      return curr;
12    });
13  }
```

On each iteration we check if the previous element is the one we're looking for.
If yes, then we return it, if not then we keep iterating unless the array has been
fully traversed. In that case we return undefined, because the element hasn't been found.

Calling *findById(people, 1)* will give us Adam, while calling *findById(people, 3)*
will result in Jack. But if we accidentally call for the element that doesn't exist on
the list, like *findById(people, 4)* we'll get the undefined value. Seems reasonable to me.

I couldn't think of a better usage than flattening collections. Think of an array of arrays,
which can happen when transforming some complex data or simply can be fetched from the backend
service. Our sample case is a list of lists of people:

```javascript
 1  var people = [
 2    [{
 3      id: 1,
 4      name: 'John'
 5    }, {
 6      id: 2,
 7      name: 'Adam'
 8    }],
 9    [{
10      id: 3,
11      name: 'Robert'
12    }]
13  ];
```

You can see there are 2 arrays inside array and we aim to reduce the complexity
of this structure by flattening it. As simple as it can get:

```javascript
 1  people = people.reduce((prev, curr) => {
 2    return prev.concat(curr);
 3  });
```

We used concat to achieve that. Now we have a structure more like our first list
without unnecessary overhead.

Last, but not least, we'll try something more difficult. Forget about the list of people
for a moment and think of one sentence. Now, calculate the amount of words which
it consists of. It can be achieved by a simple regular expression.

```javascript
 1  var text = 'Adam is taking John for a walk. John enjoys his time spent with Adam.';
 2  var words = text.match(/[A-z]+/g);
 3  var amountOfWords = words.length;
```

But what if we want to know what are the common words for this sentence? By *common*
let's assume the words that are the most frequent. In this case *Adam* and *John*
are the perfect fit, because both of them are used twice, in contrast of the rest.
Reduce to the rescue!

```javascript
 1  words = words.reduce(function(prev, curr, index) {
 2    var isPresent = prev[0].indexOf(curr);
 3
 4    if (isPresent !== -1) {
 5      prev[1][prev[0].indexOf(curr)] += 1;
 6    } else {
 7      prev[0].push(curr);
 8      prev[1].push(1);
 9    }
10
11    return prev;
12  }, [
13    [],
14    []
15  ]);
16
17  words.reduce(function(prev, curr, index, arr) {
18    arr[0].forEach(function(el, index) {
19      prev[el] = arr[1][index];
20    });
21
22    return prev;
23  }, {});
24
25  var commonWordNumber = Math.max.apply(null, words[1]);
26
27  words[0].filter(function(word, index) {
28    return words[1][index] === commonWordNumber);
29  });
```

At first, it doesn't seem simple I must say. From the beginning: by executing reduce
upon words list giving initial value of array consists of 2 empty arrays, we result
in separating words from the incidence for each word. Because of the fact we have
2 arrays with values corresponding to each other we can therefore pair them and create
an object with keys being words and values being their frequencies just so we can easier
get the values later on.

Finally, we choose the highest incidence and filter out words that are not
matching the previously found frequency.

## Combining all three together

At this point we should be familiar with *map*, *filter* and *reduce*. Our final task
is: given the following list of people:

```javascript
 1  var people = [{
 2    id: 1,
 3    name: 'John',
 4    age: 39,
 5    sports: ['basketball', 'golf']
 6  }, {
 7    id: 2,
 8    name: 'Adam',
 9    age: 29,
10    sports: ['football', 'golf']
11  }, {
12    id: 3,
13    name: 'Robert',
14    age: 84,
15    sports: ['baseball', 'basketball']
16  }, {
17    id: 4,
18    name: 'Sander',
19    age: 19,
20    sports: ['volleyball']
21  }, {
22    id: 5,
23    name: 'Cole',
24    age: 22,
25    sports: ['golf']
26  }, {
27    id: 6,
28    name: 'Bob',
29    age: 43,
30    sports: ['volleyball', 'football']
31  }];
```

We need to know what is the most popular sport played by people above 30 years old.

Try doing it on your own. One way to do that:

```javascript
 1  var playedSports = people.filter(function(person) {
 2    return person.age > 30;
 3  }).map(function(person) {
 4    return person.sports;
 5  }).reduce(function(prev, curr) {
 6    return prev.concat(curr);
 7  });
 8
 9  var mostPlayedSports = playedSports
10    .reduce(function(prev, curr) {
11      if (prev[0].indexOf(curr) !== -1) {
12        prev[1][prev[0].indexOf(curr)] += 1;
13      } else {
14        prev[0].push(curr);
15        prev[1].push(1);
16      }
17
18      return prev;
19    }, [
20      [],
21      []
22    ]);
23
24  var mostPlayedSportAmount = Math.max.apply(null, mostPlayedSports[1]);
25
26  var mostPlayedSport;
27
28  mostPlayedSports[0].forEach(function(el, index) {
29    if (mostPlayedSports[1][index] === mostPlayedSportAmount) {
30      mostPlayedSport = el;
31    }
32  });
```

## Conclusion

I can't think of a modern app that is not using map, filter and reduce
at this moment. We live in a world when simple static pages, simple HTTP request and
response patterns are not enough anymore. The real-time is actually happening
with reactive (Rx.js, Meteor) approach at the gates. I would definitely end up
defining my own map/reduce/filter functions if these didn't exist. It's far less
painful when dealing with data-intensive web apps, where operating on collections
is the main thing. Try it yourself, happy coding!
