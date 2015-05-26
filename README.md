## Example

```javascript
var co = require('co');

function *noop() {}

// taken from koa-compose
function compose(filters){
  return function *(next){
    if (!next) next = noop();

    var i = filters.length;

    while (i--) {
      next = filters[i].call(this, next);
    }

    yield *next;
  }
}

// you can create any group of filters
var filters = [

  function *(next) {
    console.log(this); // see this is 'article' passed below
    console.log('pre-1');
    yield next;
    console.log('post-1');
  },

  function *(next) {
    console.log('pre-2');
    yield next;
    console.log('post-2');
  },

  function *(next) {
    console.log('pre-3');
    yield next;
    console.log('post-3');
  }
];

// assume this is your 'article'
var article = {
  title: 'filters with genertors'
}

var it = co.wrap(compose(filters));
it.call(article);

```
