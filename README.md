
What you’re teaching

For this assignment, you’ll be teach Partials in Rails. Here are our Objectives. We frame all objectives as if you were saying the sentence "After completing this lesson, students will be able to …”. We call them the SWBATs. Here are yours:

(optional)Know which folder to put a partial if it doesn’t directly apply to the controller they are working in
(optional)Use (and know when to use) a partial from a different controller
What you’re delivering

To accomplish teaching these goals we need two things: a reading repo and a lab repo. Readings are pretty straight forward. Create a new github repo and drop in a README.md and write everything in there. For example checkout this, this, this and this.

You’ll also have to create a lab. A Lab consists of:

A README.md describing the instructions (feel free to be terse…we don’t want to give them all the answers!)
A test suite
Starter code
A solution in the solution branch
Checkout examples here, here and here

Assumptions you can make

Students are very comfortable in ruby
Students are comfortable in rspec, but a bit shaky in Capybara (if you use it)
Students know era basics
Students know the request cycle, MVC, ActiveRecord, Models really well and Controllers kinda well.
Students don’t know anything about JS
Feel free to document your assumptions of knowledge if you have any extras!



---


# Partial Views in Rails

# Overview

We'll introduce partials, which are a new kind of Rails view. We'll cover how to call
them from your views, how to pass in data, and how to use them to render collections
of models.

## Objectives

1. Understand when to use partials.
2. Use partials with collections of objects.
3. Pass local variables into partials.

## What's a partial?

A partial is a view which you can use in other views. Here's what
[the Rails documentation][rails-partials] Have to say
about them.

For example, say `hello.html.erb` looks like this:

   ```<h1>hello, <%= render 'planet' %>.</h1>```

When it encounters `<%= render 'planet' %>`, Rails will run the view in `_planet.html.erb`,
which looks like this:

```<b>earth</b>```

...and insert it into the results.

The Rails convention is that partial files are named with a leading underscore. That's why `render 'planet'`
renders the file `_planet.html.erb`. As with all things Rails, your life will be easier if you go along with the
conventions.

## Parameters

You can pass arguments into partials, just like you can pass arguments into methods.
If we edit `hello.html.erb`:

   ```<h1>hello, <%= render 'planet', object: Planet::Earth %></h1>```

We'll have access to the variable `planet` within `_planet.html.erb`:

   ```<b><%= planet.name %></b>```

This naming scheme is another Rails convention. Passing in `object` to a partial named `planet` will
set the `planet` local variable.

You can pass in arguments with whatever names you like using `locals`:

  ```<h1>hello, <%= render 'planet', object: Planet::Earth, locals: {stardate: '43989.1', distance: 1000} %>```

Which then show up as local variables in `_planet.html.erb`:

  ```<b><%= planet.name %></b>, <%= distance %> kilometers away. Today is <%= stardate %>.```

## Collections

A great and very common use for partials is to simplify view code that displays a collection
of models:

  ```<ol>
       <% @planets.each do |planet| %>
         <%= render 'planet', object: planet %>
       <% end %>
     </ol>```

This way you can reuse the `planet` partial across the list and show views.

There's a shorthand for this:

   ```<ol>
        <%= render partial: 'planet', collection: @planets %>
      </ol>```

And, Rails being Rails, there's a shorthand for this shorthand:

   ```<ol><%= render @planets %></ol>```

## Further Reading

Partials are fundamentally quite a lot like methods: they can take arguments, and they contain a bit
of functionality which you can call from many places.

There's also quite a bit of Rails magic involved, and several helpful shorthands, so I would really
recommend reading the [Rails docs on partials][rails-partials]. They're quite approchable.

[rails-partials]: http://guides.rubyonrails.org/layouts_and_rendering.html#using-partials "3.4 Using Partials"