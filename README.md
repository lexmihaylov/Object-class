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