METRO
=====

**Example**

    a->b, c, d          # => a->[b, c, [g, e, d]].join (d was expanded to '[e,g,d]')
    b->
    c->b
    d->e
    e->g,b
    f->g
    g->c
    h->
    
    a---->b<---.<---.
     `-------->c<---+---.
      `-->d--->e----'-->g<--f
    h
    
    [b,c,g,e,d,a,f,h]

### Algorithm (in words)

1. Maintain a set containing completed tasks
2. Begin at task 'a'
3. Check if 'a' has been completed
4. If 'a' completed, move to next.
5. If 'a' not completed, check its dependencies
6. If there are dependencies try to resolve them recursively
7. Repeat from Step 3. with the next task

### Algorithm (rules)

1. A task can be run if it has been resolved or has no dependencies
2. Dedupe tasks inline
3. No circular dependencies

### Objects

- **Pipeline**
  - loader              # => pipeline mapping
  - map                 # => hash from 'task' to '[..tasks..]'
  - queue
  - resolver
  - call                # => call the next task in the queue
  - exec                # => call next task and pass it return value from prev

- **Task**
  - name
  - recipient
  - message
  - params
  - optional
  - dependencies        # => run from yml file or setup schedule in code :p

### Scratch Pad

1. Use Case: Pricing
  - Get all products

### References

- [A recursive dependency algorithm](http://www.electricmonk.nl/log/2008/08/07/dependency-resolving-algorithm/)

### Concerns

- circular dependencies
- optimize algorithm (ex- maybe tasks that connect to anything could run first instead of waiting for a long chained command)
- Multithreading

* * *

blaze
=====
The purpose of this library is to allow simple CRUD access to mongodb as if it were a web service interacting via HTTP using json. Basically this 
library that maps incoming requests to running mongo process by parsing the incoming request and routing it to the appropriate handler. The handler uses
the [default Ruby driver](http://www.mongodb.org/display/DOCS/Ruby+Language+Center) to connect to mongo.  

### Use Case

MongoDB stores all kinds of data in memory and it would be extremely useful if this data was available in the form of a web service.

Similar to Riak's use case

### Configuration (config.yml)

```yaml
mongo:
  host: localhost
  post: 27017
  database: test
```

### Interface

###### Collections `/database/collections`

<table>
    <tr>
        <th>Action</th>
        <th>Verb</th>
        <th>URI</th>
        <th>Notes</th>
    </tr>
    <tr>
        <td>index</td>
        <td>GET</td>
        <td>/database/collections</td>
        <td>Return paginated results</td>
    </tr>
    <tr>
        <td>show</td>
        <td>GET</td>
        <td>/database/collections/{collection}</td>
        <td></td>
    </tr>
    <tr>
        <td>create</td>
        <td>POST</td>
        <td>/database/collections</td>
        <td></td>
    </tr>
    <tr>
        <td>update</td>
        <td>PUT</td>
        <td>/database/collections/{collection}</td>
        <td></td>
    </tr>
    <tr>
        <td>delete</td>
        <td>DELETE</td>
        <td>/database/collections/{collection}</td>
        <td></td>
    </tr>
</table>

###### Documents `/database/collection/documents`

### Misc

- zlib data transport

### References

- [Mongo Ruby driver](http://api.mongodb.org/ruby/current/file.TUTORIAL.html)
- [Rails routing](http://guides.rubyonrails.org/v2.3.11/routing.html#restful-routing-the-rails-default)

* * *

Android Tabs plugin
===================
An eclipse library project that can be used to added as a plugin to create tabs easily.

* * *

roocss
======
A templating system that allows you to define html elements as abstract objects with some properties and then genenrate HTML from them. Basically this
will allow you to create extremely testable & manageable and highly flexible layouts. roocss is based on oocss created by [Nicole Sullivan](https://github.com/stubbornella/oocss)

#### Details

An abstract object, HtmlObject, with 5 basic zones

- left column
- header
- body
- footer
- right column

where each zone itself is a nested HtmlObject with 5 zones as well. This can continue infinitely, but why do that? **The point of roocss is to simplify!**

```ruby
class HtmlObject
    attr_accessors :left_col, :header, :body, :footer, :right_col
end
```

The above code produces the following HTML:

```html
<div class="left_col">
</div>
<div class="header">
</div>
<div class="body">
</div>
<div class="footer">
</div>
<div class="right_col">
</div>
```

Here's a move visual description of what the ruby code creates:

```ruby
.-----------------.
|   |____h____|   |
|   |         |   |
| l |    b    | r |
|   |_________|   |
|   |    f    |   |
'-----------------'
```

WHICH ONES ARE OPTIONAL!?!?!?!?

As you move down the hierachy of 'HtmlObjects' you can override styles specified above. Another thing to keep in mind is that the point of such 
inheritance is to be able to get more specific the more nested an object gets, i.e., define more styles near the leaf nodes that don't exist in parents.

Lets run though a really simple example. Say we have a base layout (with a left column) defined by the following object/css:

```ruby
base = HtmlObject.new
base.left_col = HtmlObject.new
```

```css
.left_col {
    background: white;
}

.left_col .left_col {
    background: gray;
}
```

HTML produced is (*hiding header, body, footer & right_col for brevity*):

```html
<div class="left_col">
    <!-- background will be white -->
    <div class="left_col">
        <!-- background will be gray -->
    </div>
</div>
```






HOW DO I SPECIFY WHICH OPTIONAL ZONES I WANT?!?!