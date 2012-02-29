PIPELINE
--------

**Example**

    a->b, c, d          # => a->[b, c, [g, e, d]].join (d was expanded to '[e,g,d]')
    b->
    c->b
    d->e
    e->g,b
    f->g
    g->c
    h->

    [b,c,g,e,d,a,f,h]

**Algorithm (in words)**

1. Maintain a set containing completed tasks
2. Begin at task 'a'
3. Check if 'a' has been completed
4. If 'a' completed, move to next.
5. If 'a' not completed, check its dependencies
6. If there are dependencies try to resolve them recursively
7. Repeat from Step 3. with the next task

### Objects

- **Pipeline**
  - loader              # => pipeline mapping
  - map                 # => hash from 'task' to '[..tasks..]'
  - queue
  - resolve
  - call                # => call the next task in the queue

- **Task**
  - initialization?
  - recipient
  - message
  - params
  - optional

### Concerns

- Multithreading