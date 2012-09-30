## Javascript Idioms

As a programming language, Javascript has a somewhat unusual history. Created [practically overnight by a single engineer](http://www.computerworld.com.au/article/255293/a-z_programming_languages_javascript/), and initially relegated to small snippets of code embedded in web pages, the language was mostly ignored by the mainstream software development community. The resulting conventions and styles sometimes differ from those of other programming languages. A couple of these conventions -- method chaining and anonymous functions -- enjoy almost universal popularity among Javascript developers, but they can be confusing for new programmers or those with backgrounds in different languages. In this appendix, we'll take a bit of time to look at both.

### Method Chaining

_Method chaining_ helps keep Javascript code concise and understandable. It is a way of accessing an object's member functions, but it only works if the object has been defined to support it. The easiest way to understand method chaining is to compare it to traditional access methods. For example, if we didn't know about method chaining, we might code a `Ship` like this:

```javascript
Ship.Prototype = {
    setSpeed:  function(newSpeed) {
        	           this.speed = newSpeed;
               },
    setCourse: function(newCourse) {
                   this.course = newCourse;
               }
};
```

With that definition we could set sail using the following two statements:

```javascript
ship.setCourse(110);
ship.setSpeed(9);
```

There's nothing wrong with this code, but the Javascript community prefers the more concise style that method chaining permits. To support method chaining, member functions that don't naturally return any value are modified to return the object itself. So we would have:

```javascript
Ship.Prototype = {
    setSpeed:  function(newSpeed) {
                   this.speed = newSpeed;
                   return this;
               },
    setCourse: function(newCourse) {
                   this.course = newCourse;
                   return this;
               }
};
```

Notice that with this definition, when we call `setSpeed` on a ship, the function returns the ship object. The same is true for `setCourse` As a result, we can write code like this:

```javascript
ship.setCourse(110).setSpeed(9);
```

There are cases in which method chaining can get a little tricky, and, unfortunately, D3.js sets a few traps for the unwary developer. In particular, D3.js functions don't always return `this` even when the function doesn't have a natural return value. The following code illustrates the possible problem:

```javascript
var h1 = d3.selectAll("section")
    .style("background", "steelblue")
    .append("h1")
    .text("Main Title");
```

The `style()` method behaves as expected. It applies the desired style and returns the selected `section` elements, making it a perfect candidate for method chaining. The `append()` method, however, acts a little differently. It does append new `h1` elements to the selected `section` elements, but it doesn't return those selected elements. Instead, it returns the new `h1` elements that it just appended. So the `text()` method in this example applies to the `h1` elements and not the `section` elements. D3.js's choice here is deliberate; it assumes that you're likely to want to modify the appended elements, so it returns those elements for you. Nonetheless, it does break the method chaining flow.

This example shows why we usually cannot combine both the `enter()` and `exit()` functions in a single method chain. Instead we have to write something like:

```javascript
var domData = svg.selectAll("circle")
    .data(data);

domData.enter().append("circle")
    .attr("cx", x)
    .attr("cy", y)
    .attr("r", 2.5);

domData.exit().remove();
```

### Anonymous Functions

Nothing distinguishes Javascript from other languages as much as it treatment of functions. Javascript aficionados like to say "Javascript treats functions as first class objects," by which they mean that there is nothing special about Javascript functions. Anything you can do with any other object, you can also do with a Javascript function. A clear example of this freedom is Javascript's affinity for _anonymous functions_.

As you might expect, an anonymous function is a function without a name. This concept doesn't make sense in most programming languages. They typically require that functions be defined before they are used, and if you don't name a function when you define it, how can you use it later? Javascript, by contrast, lets you define *and* use a function in the same statement. There are several possible advantages to doing so:

* The namespace isn't crowded with a lot of unneeded function names.
* Functions can be defined exactly when and where they're needed, making code easier to understand and maintain.

The resulting code does look a little unusual, especially to developers new to Javascript, but the disorientation fades quickly with experience. Here's a simple D3.js example to illustrate the transition from named functions to anonymous ones:

```javascript
function receiveData(data) {
  // do something with data
};

d3.json("path/to/file.json", receiveData);
```

In the code above, we define a function to handle retrieved data, and we later reference `receiveData()` as a callback for `d3.json()`. As a general rule, callbacks are excellent opportunities for anonymous functions. Here's how we could modify the code above; 

```javascript
d3.json("path/to/file.json", function(data) {
  // do something with data
});
```

Not only is our code more concise, but it's also more maintainable. The function definition itself is a part of the `d3.json()` call, so it will be that much clearer to anyone modifying the function that the data is formatted as a JSON object. If the data format changes to CSV in the future, it will be hard to ignore any implications for the code that handles the data. Contrast that with the first example. In that version, the function definition isn't explicitly tied to the JSON format. If the format changes, it would be much easier to overlook any potential changes required in `receiveData()`.

