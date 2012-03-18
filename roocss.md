roocss - Experiments with OOCSS
===============================
A templating system that allows you to define html elements as abstract objects with some properties and then genenrate HTML from them. Basically this
will allow you to create extremely testable & manageable and highly flexible layouts. roocss is based on oocss created by [Nicole Sullivan](https://github.com/stubbornella/oocss)

#### Details

An abstract object, HtmlObject, with 5 basic zones

- left column *optional*
- header *optional*
- body **required**
- footer *optional*
- right column *optional*

where each zone itself is a nested HtmlObject with 5 zones as well. This can continue infinitely, but why do that? **The point of roocss is to simplify!**

```ruby
class HtmlObject
    attr_accessors :left_col, :header, :body, :footer, :right_col
end
```

The above class produces the following HTML:

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

Here's a picture of it:

```ruby
.-----------------.
|   |____h____|   |
|   |         |   |
| l |    b    | r |
|   |_________|   |
|   |    f    |   |
'-----------------'
```

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

.left_col > .left_col {
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

#### Goals

I refuse to proceed without goals!

- Flexibility (easy to create customizable elements)
- Manageability (concise and easy to change)
- Testability (everything can be tested)
- Semantic (layouts make sense)

#### Customizing

Creating HtmlObjects without certain zones is extremely easy. For example, the following works if you want a layout with just the left/right columns and a body:

```ruby
# Skip rendering header/footer
base = HtmlObject.new(:header, :footer)

# and a body with just a right column
base.body = HtmlObject.new(:left_col, :header, :footer)
```

The above produces:

```ruby
    |---body of
        parent----|
        
    |---body
        of
        child-|
    
.---------------------.
|   |         |   |   |
|   |         |   |   |
| l |    b    | r | r |
|   |         |   |   |
|   |         |   |   |
'---------------------'
```

Another example:

```ruby
base = HtmlObject.new(:left_col, :right_col)

base.body = HtmlObject.new(:left_col, :right_col)

base.body.header = HtmlObject.new(:left_col, :header, :footer, :right_col)
```

```html
<div class="header">
</div>
<div class="body">
    <div class="header">
        <div class="body">
        </div>
    </div>
    <div class="body">
    </div>
    <div class="footer">
    </div>
</div>
<div class="footer">
</div>
```

And if I wanted to style the above:

```css
.header { background: white; }
.header > .header { background: gray; color: red; } /* override parent styling + add specific style */

.body { background: white; }
.body > .body { background: blue; } /* override parent styling */
.body > .body > .body { background: red; } /* override parent styling again */
```

Use `.parent > .child {}` selectors in your CSS for reliable results. For common styling use `.parent .child {}`

*NOTE: The order of styles matters in CSS. Be careful!*

#### Next Steps

- Ability to add styles in code instead of in css
- Semantic elements