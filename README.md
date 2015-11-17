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

A partial is a view which you can use in other views. For example, say `hello.html.erb` looks like this:

   ```
   <h1>hello, <%= render 'planet' %>.</h1>
   ```

When it encounters `<%= render 'planet' %>`, Rails will run the view in `_planet.html.erb`:

   ```
   <b>earth</b>
   ```

...and insert it into the results.

The Rails convention is that partial files are named with a leading underscore. That's why `render 'planet'`
renders the file `_planet.html.erb`. As with all things Rails, your life will be easier if you go along with the
conventions.

## Parameters

You can pass arguments into partials, just like you can pass arguments into methods.
If we edit `hello.html.erb`:

   ```
   <h1>hello, <%= render 'planet', object: Planet::Earth %></h1>
   ```

We'll have access to the variable `planet` within `_planet.html.erb`:

   ```
   <b><%= planet.name %></b>
   ```

This naming scheme is another Rails convention. Passing in `object` to a partial named `planet` will
set the `planet` local variable.

You might be getting the sense that this is designed to make it easy to provide a default representation
for a model. And you would be right. The idea is that if you create a file named `_planet.html.erb`, and it
handles drawing a single `Planet`, Rails will make your views very terse. We'll see more of this when we use
partials for collections.

You can pass in arguments with whatever names you like using `locals`:

  ```
  <h1>hello, <%= render 'planet', object: Planet::Earth, locals: {stardate: 43989.1, distance: 1000} %>
  ```

Which then show up as local variables in `_planet.html.erb`:

  ```
  <b><%= planet.name %></b>, <%= distance %> kilometers away. Today is <%= stardate %>.
  ```

## Collections

A great and very common use for partials is to simplify view code that displays a collection
of models:

  ```
  <ol>
    <% @planets.each do |planet| %>
      <%= render 'planet', object: planet, locals: {stardate: 43989.1} %>
    <% end %>
  </ol>
  ```

This way you can reuse the `planet` partial across the list and show views.

There's a shorthand for this:

   ```
   <ol>
     <%= render partial: 'planet', collection: @planets, locals: {stardate: 43989.1}%>
   </ol>
   ```

And, Rails being Rails, there's a shorthand for this shorthand:

   ```
   <ol>
     <%= render @planets, stardate: 43989.1 %>
   </ol>
   ```

## Further Reading

Partials are fundamentally quite a lot like methods: they can take arguments, and they contain a bit
of functionality which you can call from many places.

There's also quite a bit of Rails magic involved, and several helpful shorthands, so I would really
recommend reading the [Rails docs on partials][rails-partials]. They're quite approchable.

[rails-partials]: http://guides.rubyonrails.org/layouts_and_rendering.html#using-partials "3.4 Using Partials"