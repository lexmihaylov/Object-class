# Object.class();

Is a plugin for creating class structures. Object.class is a port of the class definition in burjs. 
With this plugin it is easier to create classes, inherit classes, implement objects of methods, and call super methods.

## Installation

__Using bower__

    bower install lexmihaylov/Object-class

__Using npm__

    npm install lexmihaylov/Object-class
    
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

__Note: all static methods and properties set to the parent class will not be extended__

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

__Accessing Static methods from inside the class__

istead of typing `MyClass.staticMethod` you can use the `__self()` method from inside the class

__example:__

```javascript

var MyClass = Object.class({
    _construct: function() {
        this.myVal = "My value"
        
        console.log(this._self().staticProperty);
        console.log(this._self().staticMethod());
    }
});

MyClass.staticProperty = 'this property is static';
MyClass.staticMethod = function() {
  return 'static method called';
};

```

You can also use the `_self()` method to call `_super` like so:

```javascript
this._self()._super(this); // <=> MyClass._super(this);
```

## Example usage with Paper.js

__index.html__

```html

<!DOCTYPE html>
<html>
  <head>
    <script src="bower_components/paper/dist/paper-full.min.js"></script>
    <script src="bower_components/object-class/object-class.js"></script>
    <script src="main.js"></script>
  </head>
  <body>
    <canvas id="canvas"></canvas>
  </body>
</html>

```

__main.js__

```javascript

(function(page, dom) {

  var CustomCircle = Object.class({
    extends: paper.Path,
    _construct: function(point, radius) {
      CustomCircle._super(this);
      this.center = {
        x: point.x,
        y: point.y
      }

      this.radius = radius;
      this.selected = true;
      this.closed = true;

      this.drawSegments();

      this.smooth();
    },
    // class methods
    prototype: {
      drawSegments: function() {
        for(var ang = 315; ang >= 0; ang -= 45) {
          this.add(
            {
              x: this.center.x + this.radius*Math.cos(this.degToRad(ang)),
              y: this.center.y + this.radius*Math.sin(this.degToRad(ang))
            }
          );
        }
      },

      /**
       * converts degrees to radians
       */
      degToRad: function(deg) {
        return deg * (Math.PI / 180);
      }
    }
  });

  page.onload = function() {
    var canvas = dom.getElementById('canvas');

    canvas.width = 600;
    canvas.height = 600;

    paper.setup(canvas);

    var circle = new CustomCircle({x: 300, y: 300}, 150);

    paper.view.draw();
  };

})(window, document);

```
