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