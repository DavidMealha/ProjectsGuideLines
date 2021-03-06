# Javascript & jQuery Guidelines #

## Types of Variables ##

Until ES5 there was only the reserve word `var`, since then, from ES6+ there's also `let` and `const`.

* `const` means that the identifier can't be reassigned, doesn't mean that's it's immutable.
* `let` allows the variable to be reassigned, and only allows access in the context, while `var` is a global variable, acessible everywhere in the program.

## Creating an JS Object ##

### ES5 ###

#### Using a function ####

```javascript
function Car (model, year) {
    this.model = model;
    this.year = year;
}
 
Apple.prototype.getInfo = function() {
    return 'My ' + this.model + ' is from ' + this.year;
};
```

#### Using object properties ####

```javascript
var car = {
    model: 'Ferrari',
    year: 2017,
    getInfo: function () {
    	return 'My ' + this.model + ' is from ' + this.year;
	}
}
```

### ES6 ###