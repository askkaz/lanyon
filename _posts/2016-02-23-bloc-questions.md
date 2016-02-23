---
layout: post
title: BLOC SET Assessment
---

I'm applying for a role as a mentor with [bloc.io](http://bloc.io). They are one of the more comprehensive online SW bootcamp platforms. A number (multitude, really) of SW bootcamps have emerged over the past few years. This is a great trend. Completing a front end nanodegree through [Udacity](http://udacity.com) really helped me come up to speed with the current state of the web. The projects I completed helped me build a portfolio and ultimately make the transition to this industry. However, getting exposed to web technologies is something that one can do very easily for free online. The main value for me from this program was the interaction with others, including code reviews with SW devs at Udacity. Getting code to run and writing good code are very different things.

The [bloc.io](http://bloc.io) programs have a higher sticker price than other bootcamps, but their value really comes from teaching people to write great code. Their approach to this is a model where students are paired with paid mentors. In addition to learning software languages, students develop good coding habits. This one-on-one approach helps students become not only successful problem solvers in the selected language, but also effective employees who work well with others.

As part of the application, I need to submit answers to a selection of student questions:

## JavaScript

### Question

Hey mentor,

I was working through an assignment last night and came across a bug. I'm so confused!

The text in the alert is showing up as "undefined" instead of either "A Unicorn", "A hug", or "Fresh Laundry" and I'm not quire sure what's going on. Can you point me in the right direction?

-Student

```html
<button id="btn-0">Button 1!</button>
<button id="btn-1">Button 2!</button>
<button id="btn-2">Button 3!</button>

<script type="text/javascript">
  var prizes = ['A Unicorn!', 'A Hug!', 'Fresh Laundry!'];
  for (var btnNum = 0; btnNum < prizes.length; btnNum++) {
      // for each of our buttons, when the user clicks it...
      document.getElementById('btn-' + btnNum).onclick = function() {
          // tell her what she's won!
          alert(prizes[btnNum]);
      };
  }
</script>
```

### Answer

Student,

This is one of the things that I found less than intuitive when I was learning JavaScript. The main issue you are coming across here is "closures". Let's take a look.

As you mention, when I run your code and click a button, I get 'undefined'. My first thought - are we accessing an element that does not exist? So I modify the alert to:

```html
alert(btnNum);
```
And we see that no matter what button is clicked, `btnNum` is `3`. This is the last value that `btnNum` is set to before the `for` loop exits. Why? The function that includes the `alert` doesn't run until the button is clicked. Since `btnNum` gets incremented each run through the `for` loop, it will be `3` by the time one of the buttons is clicked. You can even modify the value of `btnNum` in the console and see the alerts change accordingly.

There are a couple ways this can be solved. You could parse the id of the clicked button and choose the correct message to display. A more mature approach is to enclose the alert function in a what is called a "closure", so that the alert message associated with each button is executed when it is created. This ensures the correct message is displayed on click.

Here is the modified version of the script:

```javascript
function alertPrize(prizeNum) {
    return function() {
        alert(prizes[prizeNum]);
    };
}
for (var btnNum = 0; btnNum < prizes.length; btnNum++) {
  // for each of our buttons, when the user clicks it...
  document.getElementById('btn-' + btnNum).onclick =
      // tell her what she's won!
      alertPrize(btnNum);
}
```

Instead of assigning a function dependent on `btnNum` to each button, we are assigning a function that already has the appropriate number assigned. Technically, this solves your problem. However, note that it is still dependent on `prizes` - if you edit the `prizes` variable in the console, it will change the alert message.

If we modify the code to:

```javascript
function alertStatic(message) {
    return function() {
        alert(message);
    };
}
for (var btnNum = 0; btnNum < prizes.length; btnNum++) {
  // for each of our buttons, when the user clicks it...
  document.getElementById('btn-' + btnNum).onclick =
      // tell her what she's won!
      alertStatic(prizes[btnNum]);
}
```

You will now be unable to change the alert messages by modifying `btnNum` or `prizes` in the console. We also have a function that might be useful elsewhere in our site.

## SQL

### Question

Hey mentor,

Thanks for the lecture yesterday, it really helped me get motivated again. I've been working through this assignment about databases and I'm confused about "SQL injection." What does that mean and how does it work? How do I prevent it in my apps?

Thanks!

-Student

### Answer

Student,

Glad you enjoyed the lecture. Basically, SQL injection is when a user enters code into an input on your app and it executes malicious SQL queries on your database. This can be done by modifying urls, text input fields, etc. Most of the Rails framework will protect you from these sorts of injections, but some of the direct database access functions do not. You can find a good listing of these functions [here](http://rails-sqli.org/). A good habit to get into (which will also protect you when using these direct database functions) is to "escape" user input or avoid using the user input directly as part of the database functions.

As an example, say we are trying to find if a media object exists given a user supplied media id. This approach is vulnerable to an SQL injection:

```ruby
Media.exists? params[:media_id]
```

We are expecting an id here. Instead of checking it directly, we can map the user input to an id explicitly. This function is safe.

```ruby
Media.exists? :id => params[:media_id]
```

## Angular/jQuery

### Question

Hey mentor,

I'm really excited about starting on learning Angular. I'm having a problem understanding what angular is. Up until this point we've used jQuery, which we've called a library. Now we are using Angular, which is a framework. What's the difference? What are the drawbacks/benefits of using a framework like Angular vs a library like jQuery?

-Student

### Answer

Student,

Angular is one of [many](https://en.wikipedia.org/wiki/Comparison_of_web_frameworks) available app frameworks. We've chosen Angular for this course because it is one of the more popular and in-demand JavaScript frameworks.

A web framework is something that helps a programmer organize their application. As developers, one habit that we try to follow is "separation of concerns" - essentially, everything has its place. If you look at other frameworks, they are usually listed under a specific software architecture pattern.
These include:
<ul>
<li>MVC - model, view, controller</li>
<li>MVVM - model, view, viewmodel (seriously)</li>
<li>MVP - model, view, presenter</li>
</ul>
There are some differences between these, but each one seeks to separate apps into 3 main parts.
These three parts are:
<ul>
<li>User interaction with the app</li>
<li>Storage of the underlying data</li>
<li>Translation between the first two parts</li>
</ul>
Functions are placed within the app based on which part they are contributing to. There are a number of benefits to following these conventions. These include easier maintainability and portability of code.

jQuery is a library that simplifies manipulation of the objects within a webpage. It is possible to build an app simply using jQuery (without a framework), but it is going to get messy fast. In fact, Angular uses jQuery as a dependency (though it can be replaced with jQLite if you want to save space), so we will actually be using both for the remainder of the project.

## Algorithms

### Question

Hey mentor,

I hope you're doing well. I'm really enjoying learning algorithms, but I'm pretty confused by this Big O Notation thing. Can you explain how I'm supposed to figure out which notation my algorithm uses?

-Student

### Answer

Student,

Big O notation is one of my favorite things to think about when designing an algorithm. We use this notation to give an easy way to compare the efficiency of algorithms, given an input size (_n_). Note that we simplify an algorithm's notation to its "worst" order. As usage of the algorithm approaches an infinite number of calls, the slowest parts of the algorithm will limit execution time. I'll go over a few of the more common orders so that you can get a feel for them. I've presented these in increasing order - meaning they get slower as I go on.

#### O(1) - Constant Time

An algorithm that is O(1), means that no matter the input size, it will always take the same amount of time. A good example of this is accessing the first element in a list. No matter how long (_n_) the list is, you are always just grabbing the first element.

#### O(_n_) - Linear Time

An algorithm that is O(_n_) means that the time increases linearly as the size of the input increases. An example of this is accessing every element in a list. If a list of length 10 takes 10 milliseconds to process, it will take 100 milliseconds to process a list of length 100.

#### O(_n_<sup>2</sup>) - Quadratic Time

An algorithm that is O(_n_<sup>2</sup>) means that the time increases quadratically as the size of the input increases. One example is generating every combination of two elements within a list in a loop:

```python
for i in list:
    for j in list:
        print(i+j)
```

If the list is _n_ long, we will run through the outer loop _n_ times and each of those goes through the inner loop _n_ times as well, resulting in _n_ * _n_ or _n_<sup>2</sup> times.

Note that for any of these, we drop any constant associated with the execution time. As an example:

```python
for i in list:
    print(i)
for j in list:
    print(j)
```

We are going through the list twice, so you might think to list execution time as O(2_n_). However, we drop the constant `2` because at scale, because the time is dominated by the "order". This function is O(_n_).

## CSS

Hey mentor,

I'm having a really hard time with some CSS. I've been given a PSD in an assignment. I know what the document should [look like](https://www.dropbox.com/s/figypqiy92q0bz3/Screenshot%202015-12-17%2017.08.46.png?dl=0), but I'm getting [this](https://www.dropbox.com/s/hrx3xfb135243vf/Screenshot%202015-12-17%2017.09.28.png?dl=0) instead.

Can you help me with this? Here is my code!

-Student

```html
<header>

</header>
<section class="pricing">
  <div class="pricing-tiers">
    <div class="tier">
      <h1>The plan</h1>
      <ul>
        <li>You get all the things</li>
        <li>plus more!!</li>
      </ul>
    </div>

    <div class="tier">
      <h1>The plan</h1>
      <ul>
        <li>You get all the things</li>
        <li>plus more!!</li>
      </ul>
    </div>

    <div class="tier">
      <h1>The plan</h1>
      <ul>
        <li>You get all the things</li>
        <li>plus more!!</li>
      </ul>
    </div>
  </div>
</section>
```

```css
header {
  width: 100%;
  height: 60px;
  background-color: #333;
}

.pricing {
  width: 100%;
  padding: 20px;
  background-color: tomato;
}

.pricing-tiers {
  width: 960px;
  margin: 0px auto;
  background-color: #ddd;
}

.pricing-tiers .tier {
  width: 260px;
  margin: 20px;
  padding: 10px;
  background-color: white;
  float: left;
}

.tier h1 {
  font-family: "Helvetica", sans-serif;
  font-weight: bold;
  font-size: 2em;
  margin-bottom: 20px;
  text-align: center;
}

.pricing-tiers > .tier:last-child {
  margin-right: 0px;
}
```

### Answer

Student,

CSS can be a real bear sometimes. There are a few small changes that we need to make to fix this layout. I encourage you to open the inspector tools and make the changes one by one so you can see the effect. Once you get it how you like, you can make the same changes to your CSS.

1. The first thing we'd like to have is the orange and gray outlines to stretch around our three boxes. This can be fixed by adding `display: inline-block;` to the `.pricing-tiers` selector. This has to do with the fact that `inline-block` elements and `block` (what it was before) elements handle height differently. Essentially, as a `block` element, it sees no direct text in the div, so it assumes a height of zero. There is a good write-up of some of the benefits and drawbacks of inline-block [here](http://designshack.net/articles/css/whats-the-deal-with-display-inline-block/). It should work well for this application though.

2. Now the heights look right, but it is no longer centered. We can fix this by adding `text-align: center;` to the `.pricing` selector. I also changed the `padding-left` and `padding-right` values to 0, because in my browser, it was rendering wider than the header.

3. The text inside the smaller boxes is centered, but we want them aligned left. For this, we'll use `text-align: left;`. We could add this as a property to each box, but instead, we can add it to the `.pricing-tiers` selector and the boxes will all inherit it.

4. Lastly, lets get rid of those `ul` bullets. We can do this with `list-style-type: none;`. Instead of applying it to `ul`, lets apply it to `.tier ul` in case we decide we need the bullets elsewhere on the page.

The best way to learn CSS is really experience, and playing around with sites using the browser inspector.
