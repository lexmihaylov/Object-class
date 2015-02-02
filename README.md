# Object.class();

Is a plugin for creating class structures. Object.class is a port of the class definition in burjs. 
With this plugin it is easear to create classes, inherit classes, implement objects of methods, and call super methods.

## Installation

__Using bower__

    bower install lexmihaylov/Object.class

__Using npm__

    npm install lexmihaylov/Object.class
    
## Usage

__Creating a class__

```javascript

var MyClass = Object.class({
    _construct: function() {
        this.myVal = "My value"
    }
});

MyClass.prototype.getMyVal = function() {
    return this.myVal;
};

var obj = new MyClass();

console.log(obj.getMyVal()); // => "My Value"

```

__Extending a class__

```javascript

var MyChildClass = Object.class({
    extends: MyClass,
    _construct: function() {
        MyChildClass._super(this);
    }
});

var obj = new MyChildClass();

console.log(obj.getMyVal()) // => "My Value"

```

__Overriding methods__

```javascript

var MyChildClass = Object.class({
    extends: MyClass,
    _construct: function() {
        MyChildClass._super(this);
    }
});

MyChildClass.prototype.getMyVal = function() {

    var val = MyChildClass._super(this, 'getMyVal');
    
    return val + ' [called from child class] ';

};

var obj = new MyChildClass();

console.log(obj.getMyVal()) // => "My Value [called from child class] "

```

__Super method__

the super method accepts three parameters
 * `context` - is the context in which the method will be executed
 * `method` (optional) - method name to execute. If a method is not supplied then the class constructor will be executed
 * `params` (optional) - array of parameters to pass to the super method

__examples:__

```javascript
Class._super(this) // will call the parent class' constructor without any arguments
Class._super(this, [arg1, arg2]) // will call parent class' constructor with arg1 and arg2
var result = Class._super(this, 'parentMethod', [arg1, arg2]); // will call parent method with arg1 and arg2 as input arguments
```