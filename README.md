backbone-form_builder
=====================

`form_builder` makes creating Backbone forms super easy.

It's designed to work with CoffeeScript templates and
I've tested it with `haml-coffee`, `skim`, and `eco`.
Let me know if you use it with a different environment and I'll
add it to the examples.

Usage
-----

Require `form_builder` after backbone.

Using [`haml-coffee`](https://github.com/netzpirat/haml-coffee):

```hamlc
!= form_for @current_user, autocomplete: 'off', (f) ->
  != f.text_field 'username', autocapitalize: 'off'
  != f.password_field 'password'

  != f.submit "Sign In"
```

Using [`skim`](https://github.com/jfirebaugh/skim):

```skim
== form_for @current_user, autocomplete: 'off', (f) ->
  == f.text_field 'username', autocapitalize: 'off'
  == f.password_field 'password'

  == f.submit "Sign In"
```

Using [`eco`](https://github.com/sstephenson/eco):

```eco
<%- form_for @current_user, autocomplete: 'off', (f) -> %>
  <%- f.text_field 'username', autocapitalize: 'off' %>
  <%- f.password_field 'password' %>
  <%- f.submit "Sign In" %>
<% end %>
```

The `error` class is added to fields with validation errors.
You can display the errors with `errors_for`:

```skim
== form_for @current_user, (f) ->
  == f.errors_for 'username'
  == f.text_field 'username'
```

The available methods are:

* `f.input(type, attribute, options = {})`
* `f.text_field(attribute, options = {})`
* `f.checkbox(attribute, options = {})`
* `f.password_field(attribute, options = {})`
* `f.label(attribute, body = attribute, options = {})`
* `f.select(attribute, choices = {}, options = {})`
* `f.submit(value, options = {})`

You can customize html attributes by passing options:

```skim
== form_for @current_user, autocomplete: 'off', (f) ->
  == f.text_field 'username', id: 'username'
```

`select` takes a `choices` object to create a series of `<option>` tags:

```skim
== f.select "color", red: "Red", blue: "Blue"
```

Renders as:

```html
<select name="color">
  <option value="red">Red</option>
  <option value="blue">Blue</option>
</select>
```

Nested attributes
-----

*(tested with [`Backbone-relational`](https://github.com/PaulUithol/Backbone-relational))*

Now, you can use nested attributes for **one-to-one** relationships. For instance, let's suppose you have a client model that has a role relationship and the role has an attribute called *name*. You must be able to access the user's role like this: `user.get("role")`, and it should return a *Role* model. You can do a `text_field` for that role name in this way:

`f.text_field "role[name]"`

For the value, the plugin will call `user.get("role").get("name")`
