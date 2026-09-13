## Templating

```jinja2
Hello {{ name }}!
```

It contains a single variable called `name`, which is replaced with a dynamic value during rendering. When the template is rendered, the template engine must be provided with a value for the variable `name`. For instance, if we provide the variable `name="vautia"` to the rendering function, the template engine will generate the following string:

```txt
Hello vautia
```

```jinja2
{% for name in names %}
Hello {{ name }}!
{% endfor %}
```

The template contains a `for-loop` that loops over all elements in a variable `names`. As such, we need to provide the rendering function with an object in the `names` variable that it can iterate over. For instance, if we pass the function with a list such as `names=["vautia", "21y4d", "Pedant"]`, the template engine will generate the following string:

```txt
Hello vautia! 
Hello 21y4d! 
Hello Pedant!
```
