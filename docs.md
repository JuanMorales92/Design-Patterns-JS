# Design Patterns JS

**[Behavioral](#behavioral)**
* [Chain Of Resp](#chain-of-resp)
* [Command](#command)
* [Interpreter](#interpreter)
* [Iterator](#iterator)
* [Mediator](#mediator)
* [Memento](#memento)
* [Observer](#observer)
* [State](#state)
* [Strategy](#strategy)
* [Template](#template)
* [Visitor](#visitor)

**[Creational](#creational)**
* [Abstract Factory](#abstract-factory)
* [Builder](#builder)
* [Factory Method](#factory-method)
* [Prototype](#prototype)
* [Singleton](#singleton)

**[Structural](#structural)**
* [Adapter](#adapter)
* [Decorator](#decorator)
* [Facade](#facade)
* [Flyweight](#flyweight)
* [Proxy](#proxy)



## behavioral
### Chain Of Resp
##### chain-of-resp-es6.js
```Javascript
class ShoppingCart {
    constructor() {
        this.products = [];
    }

    addProduct(p) {
        this.products.push(p);
    };
}

class Discount {
    calc(products) {
        let ndiscount = new NumberDiscount();
        let pdiscount = new PriceDiscount();
        let none = new NoneDiscount();
        ndiscount.setNext(pdiscount);
        pdiscount.setNext(none);
        return ndiscount.exec(products);
    };
}

class NumberDiscount {
    constructor() {
        this.next = null;
    }

    setNext(fn) {
        this.next = fn;
    };

    exec(products) {
        let result = 0;
        if (products.length > 3)
            result = 0.05;

        return result + this.next.exec(products);
    };
}

class PriceDiscount{
    constructor() {
        this.next = null;
    }

    setNext(fn) {
        this.next = fn;
    };

    exec(products) {
        let result = 0;
        let total = products.reduce((a, b)=> a + b);

        if (total >= 500)
            result = 0.1;

        return result + this.next.exec(products);
    };
}

class NoneDiscount {
    exec() {
        return 0;
    };
}

export { ShoppingCart , Discount };

```
##### chain-of-resp.js
```Javascript
function ShoppingCart() {
    this.products = [];

    this.addProduct = function(p) {
        this.products.push(p);
    };
}

function Discount() {
    this.calc = function(products) {
        var ndiscount = new NumberDiscount();
        var pdiscount = new PriceDiscount();
        var none = new NoneDiscount();

        ndiscount.setNext(pdiscount);
        pdiscount.setNext(none);

        return ndiscount.exec(products);
    };
}

function NumberDiscount() {
    this.next = null;
    this.setNext = function(fn) {
        this.next = fn;
    };

    this.exec = function(products) {
        var result = 0;
        if (products.length > 3)
            result = 0.05;

        return result + this.next.exec(products);
    };
}

function PriceDiscount() {
    this.next = null;
    this.setNext = function(fn) {
        this.next = fn;
    };
    this.exec = function(products) {
        var result = 0;
        var total = products.reduce(function(a, b) {
            return a + b;
        });

        if (total >= 500)
            result = 0.1;

        return result + this.next.exec(products);
    };
}

function NoneDiscount() {
    this.exec = function() {
        return 0;
    };
}

module.exports = [ShoppingCart, Discount];

```

### Command
##### command.js
```Javascript
function Cockpit(command) {
    this.command = command;
}
Cockpit.prototype.execute = function() {
    this.command.execute();
};

function Turbine() {
  this.speed = 0;
  this.state = false;
}

Turbine.prototype.on = function() {
    this.state = true;
    this.speed = 100;
};

Turbine.prototype.off = function() {
    this.speed = 0;
    this.state = false;
};

Turbine.prototype.speedDown = function() {
    if (!this.state) return;

    this.speed -= 100;
};

Turbine.prototype.speedUp = function() {
    if (!this.state) return;

    this.speed += 100;
};


function OnCommand(turbine) {
    this.turbine = turbine;
}
OnCommand.prototype.execute = function() {
    this.turbine.on();
};

function OffCommand(turbine) {
    this.turbine = turbine;
}
OffCommand.prototype.execute = function() {
    this.turbine.off();
};

function SpeedUpCommand(turbine) {
    this.turbine = turbine;
}
SpeedUpCommand.prototype.execute = function() {
    this.turbine.speedUp();
};

function SpeedDownCommand(turbine) {
    this.turbine = turbine;
}
SpeedDownCommand.prototype.execute = function() {
    this.turbine.speedDown();
};


module.exports = [Cockpit, Turbine, OnCommand, OffCommand, SpeedUpCommand, SpeedDownCommand];

```
##### command_es6.js
```Javascript
class Cockpit {
    constructor(command) {
        this.command = command;
    }
    execute() {
        this.command.execute();
    }
}

class Turbine {
    constructor() {
        this.state = false;
    }
    on() {
        this.state = true;
    }
    off() {
        this.state = false;
    }
}

class OnCommand {
    constructor(turbine) {
        this.turbine = turbine;
    }
    execute() {
        this.turbine.on();
    }
}

class OffCommand {
    constructor(turbine) {
        this.turbine = turbine;
    }
    execute() {
        this.turbine.off();
    }
}

export { Cockpit, Turbine, OnCommand, OffCommand };


```

### Interpreter
##### interpreter.js
```Javascript
function Sum(left, right) {
    this.left = left;
    this.right = right;
}

Sum.prototype.interpret = function() {
   return this.left.interpret() + this.right.interpret();
};

function Min(left, right) {
    this.left = left;
    this.right = right;
}

Min.prototype.interpret = function() {
   return this.left.interpret() - this.right.interpret();
};

function Num(val) {
    this.val = val;
}

Num.prototype.interpret = function() {
    return this.val;
};

module.exports = [Num, Min, Sum];

```
##### interpreter_es6.js
```Javascript
class Sum {
    constructor(left, right) {
        this.left = left;
        this.right = right;
    }

    interpret() {
        return this.left.interpret() + this.right.interpret();
    }
}

class Min {
    constructor(left, right) {
        this.left = left;
        this.right = right;
    }

    interpret() {
        return this.left.interpret() - this.right.interpret();
    }
}


class Num {
    constructor(val) {
        this.val = val;
    }

    interpret() {
        return this.val;
    }
}


export { Num, Min, Sum };

```

### Iterator
##### iterator.js
```Javascript
function Iterator(el) {
    this.index = 0;
    this.elements = el;
}

Iterator.prototype = {
    next: function() {
        return this.elements[this.index++];
    },
    hasNext: function() {
        return this.index < this.elements.length;
    }
};

module.exports = Iterator;

```
##### iterator_es6.js
```Javascript
class Iterator {
    constructor(el) {
        this.index = 0;
        this.elements = el;
    }

    next() {
        return this.elements[this.index++];
    }

    hasNext() {
        return this.index < this.elements.length;
    }
}

export default Iterator;

```

### Mediator
##### mediator.js
```Javascript
function TrafficTower() {
    this.airplanes = [];
}

TrafficTower.prototype.requestPositions = function() {
    return this.airplanes.map(function(airplane) {
        return airplane.position;
    });
};

function Airplane(position, trafficTower) {
    this.position = position;
    this.trafficTower = trafficTower;
    this.trafficTower.airplanes.push(this);
}

Airplane.prototype.requestPositions = function() {
    return this.trafficTower.requestPositions();
};

module.exports = [TrafficTower, Airplane];

```
##### mediator_es6.js
```Javascript
class TrafficTower {
    constructor() {
        this.airplanes = [];
    }

    requestPositions() {
        return this.airplanes.map(airplane => {
            return airplane.position;
        });
    }
}

class Airplane{
    constructor(position, trafficTower) {
        this.position = position;
        this.trafficTower = trafficTower;
        this.trafficTower.airplanes.push(this);
    }

    requestPositions() {
        return this.trafficTower.requestPositions();
    }
}


export { TrafficTower, Airplane };

```

### Memento
##### memento.js
```Javascript
function Memento(value) {
    this.value = value;
}

var originator = {
    store: function(val) {
        return new Memento(val);
    },
    restore: function(memento) {
        return memento.value;
    }
};

function Caretaker() {
    this.values = [];
}

Caretaker.prototype.addMemento = function(memento) {
    this.values.push(memento);
};

Caretaker.prototype.getMemento = function(index) {
    return this.values[index];
};

module.exports = [originator, Caretaker];

```
##### memento_es6.js
```Javascript
class Memento {
    constructor(value) {
        this.value = value;
    }
}

const originator = {
    store: function(val) {
        return new Memento(val);
    },
    restore: function(memento) {
        return memento.value;
    }
};

class Caretaker {
    constructor() {
        this.values = [];
    }

    addMemento(memento) {
        this.values.push(memento);
    }

    getMemento(index) {
        return this.values[index];
    }
}



export { originator, Caretaker };

```

### Observer
##### observer.js
```Javascript
function Product() {
    this.price = 0;
    this.actions = [];
}

Product.prototype.setBasePrice = function(val) {
    this.price = val;
    this.notifyAll();
};

Product.prototype.register = function(observer) {
    this.actions.push(observer);
};

Product.prototype.unregister = function(observer) {
    this.actions.remove.filter(function(el) {
        return el !==  observer;
    });
};

Product.prototype.notifyAll = function() {
    return this.actions.forEach(function(el) {
        el.update(this);
    }.bind(this));
};

var fees = {
    update: function(product) {
        product.price = product.price * 1.2;
    }
};

var proft = {
    update: function(product) {
        product.price = product.price * 2;
    }
};

module.exports = [Product, fees, proft];

```
##### observer_es6.js
```Javascript
class Product {
    constructor() {
        this.price = 0;
        this.actions = [];
    }

    setBasePrice(val) {
        this.price = val;
        this.notifyAll();
    }

    register(observer) {
        this.actions.push(observer);
    }

    unregister(observer) {
        this.actions.remove.filter(function(el) {
            return el !== observer;
        });
    }

    notifyAll() {
        return this.actions.forEach(function(el) {
            el.update(this);
        }.bind(this));
    }
}

class fees {
    update(product) {
        product.price = product.price * 1.2;
    }
}

class proft {
    update(product) {
        product.price = product.price * 2;
    }
}

export { Product, fees, proft };

```

### State
##### state.js
```Javascript
function Order() {
    this.state = new WaitingForPayment();

    this.nextState = function() {
        this.state = this.state.next();
    };
}


function WaitingForPayment() {
    this.name = 'waitingForPayment';
    this.next = function() {
        return new Shipping();
    };
}

function Shipping() {
    this.name = 'shipping';
    this.next = function() {
        return new Delivered();
    };
}

function Delivered() {
    this.name = 'delivered';
    this.next = function() {
        return this;
    };
}

module.exports = Order;

```
##### state_es6.js
```Javascript
class OrderStatus {
    constructor(name, nextStatus) {
        this.name = name;
        this.nextStatus = nextStatus;
    }

    next() {
        return new this.nextStatus();
    }
}

class WaitingForPayment extends OrderStatus {
    constructor() {
        super('waitingForPayment', Shipping);
    }
}

class Shipping extends OrderStatus {
    constructor() {
        super('shipping', Delivered);
    }
}


class Delivered extends OrderStatus {
    constructor() {
        super('delivered', Delivered);
    }
}

class Order {
    constructor() {
        this.state = new WaitingForPayment();
    }

    nextState() {
        this.state = this.state.next();
    };
}

export default Order;

```

### Strategy
##### strategy.js
```Javascript
function ShoppingCart(discount) {
    this.discount = discount;
    this.amount = 0;
}

ShoppingCart.prototype.setAmount = function(amount) {
    this.amount = amount;
};

ShoppingCart.prototype.checkout = function() {
   return this.discount(this.amount);
};

function guestStrategy(amount) {
    return amount;
}

function regularStrategy(amount) {
    return amount * 0.9;
}

function premiumStrategy(amount) {
    return amount * 0.8;
}

module.exports = [ShoppingCart, guestStrategy, regularStrategy, premiumStrategy];

```
##### strategy_es6.js
```Javascript
class ShoppingCart {

    constructor(discount) {
        this.discount = discount;
        this.amount = 0;
    }

    checkout() {
        return this.discount(this.amount);
    }

    setAmount(amount) {
        this.amount = amount;
    }
}

function guestStrategy(amount) {
    return amount;
}

function regularStrategy(amount) {
    return amount * 0.9;
}

function premiumStrategy(amount) {
    return amount * 0.8;
}

export { ShoppingCart, guestStrategy, regularStrategy, premiumStrategy };

```

### Template
##### template.js
```Javascript
function Tax() {}

Tax.prototype.calc = function(value) {
   if (value >= 1000)
       value = this.overThousand(value);

    return this.complementaryFee(value);
};

Tax.prototype.complementaryFee = function(value) {
    return value + 10;
};


function Tax1() {}
Tax1.prototype = Object.create(Tax.prototype);

Tax1.prototype.overThousand = function(value) {
    return value * 1.1;
};


function Tax2() {}
Tax2.prototype = Object.create(Tax.prototype);

Tax2.prototype.overThousand = function(value) {
    return value * 1.2;
};

module.exports = [Tax1, Tax2];

```
##### template_es6.js
```Javascript
class Tax {
    calc(value) {
        if (value >= 1000)
            value = this.overThousand(value);

        return this.complementaryFee(value);
    }

    complementaryFee(value) {
        return value + 10;
    }

}

class Tax1 extends Tax {
    constructor() {
        super();
    }
    overThousand(value) {
        return value * 1.1;
    }
}

class Tax2 extends Tax {
    constructor() {
        super();
    }
    overThousand(value) {
        return value * 1.2;
    }
}

export { Tax1, Tax2 };

```

### Visitor
##### visitor.js
```Javascript
function bonusVisitor(employee) {
    if (employee instanceof Manager)
        employee.bonus = employee.salary * 2;
    if (employee instanceof Developer)
        employee.bonus = employee.salary;
}

function Employee() {
    this.bonus = 0;
}

Employee.prototype.accept = function(visitor) {
    visitor(this);
};

function Manager(salary) {
    this.salary = salary;
}

Manager.prototype = Object.create(Employee.prototype);

function Developer(salary) {
    this.salary = salary;
}

Developer.prototype = Object.create(Employee.prototype);


module.exports = [Developer, Manager, bonusVisitor];

```
##### visitor_es6.js
```Javascript
function bonusVisitor(employee) {
    if (employee instanceof Manager)
        employee.bonus = employee.salary * 2;
    if (employee instanceof Developer)
        employee.bonus = employee.salary;
}

class Employee {
    constructor(salary) {
        this.bonus = 0;
        this.salary = salary;
    }

    accept(visitor) {
        visitor(this);
    }
}

class Manager extends Employee {
    constructor(salary) {
        super(salary);
    }
}

class Developer extends Employee {
    constructor(salary) {
        super(salary);
    }
}

export { Developer, Manager, bonusVisitor };

```


## creational
### Abstract Factory
##### abstract-factory_es6.js
```Javascript

class GuiFactory {
  constructor() {
  }

  typeOf() {
    return "IGuiFactory";
  }

  renderBtn() {
    return this.Btn.render();
  }

  renderTitle() {
    return this.Title.render();
  }

  onClick() {
    return "Hi";
  }
}

class WinFactory extends GuiFactory {
  constructor() {
    super();
    this.Btn = new WinBtn();
    this.Title = new WinTitle();
  }
}

class MacFactory extends GuiFactory {
  constructor() {
    super();
    this.Btn = new MacBtn();
    this.Title = new MacTitle();
  }
}

class IBtn {
    render() {
        throw err("Not Implemented");
    }
}

class ITitle {
    render() {
        throw err("Not Implemented");
    }
}

class WinBtn extends IBtn {
  render() {
    return "<win>Button</win>"
  }
}

class WinTitle extends ITitle {
  render() {
    return "<win>Title</win>"
  }
}

class MacBtn extends IBtn {
  render() {
    return "<mac>Button</mac>"
  }
}

class MacTitle extends ITitle {
  render() {
    return "<mac>Title</mac>"
  }
}


export { GuiFactory, WinFactory, MacFactory };

```

### Builder
##### builder_es6.js
```Javascript
class Request {
    constructor() {
        this.url = '';
        this.method = '';
        this.payload = {};
    }
}

class IBuilder {
    constructor() {
    }

    forUrl(url) {
      throw "Not Implemented";
    }

    useMethod(method) {
      throw "Not Implemented";
    }

    payload(payload) {
      throw "Not Implemented";
    }

}

class RequestBuilder extends IBuilder{
    constructor() {
        super();
        this.request = new Request();
    }

    forUrl(url) {
        this.request.url = url;
        return this;
    }

    useMethod(method) {
        this.request.method = method;
        return this;
    }

    payload(payload) {
        this.request.payload = payload;
        return this;
    }

    build() {
        return this.request;
    }

}

class DocBuilder extends IBuilder{
    constructor() {
        super();
        this.urlDoc = "";
        this.methodDoc = "";
        this.payloadDoc = "";
    }

    forUrl(url) {
        this.urlDoc = url;
        return this;
    }

    useMethod(method) {
        this.methodDoc = method;
        return this;
    }

    payload(payload) {
        this.payloadDoc = payload;
        return this;
    }

    build() {
        return `<url>${this.urlDoc}</url><method>${this.methodDoc}</method><payload>${this.payloadDoc}</payload>`;
    }

}

export { RequestBuilder, DocBuilder, IBuilder, Request };

```

### Factory Method
##### factory-method_es6.js
```Javascript
class Employee {
  constructor(employeeId) {
    this.employeeId = employeeId;
  }
}


class EmployeeFactory {
  constructor() {
    this.employees = {};
  }

  findOrCreateEmployee(id) {
    if (this.employees[id] === undefined)
        this.employees[id] = new Employee(id);
    return this.employees[id];
  }

  getNumberOfEmployees() {
    return Object.keys(this.employees).length;
  }

}

export { EmployeeFactory, Employee };

```

### Prototype
##### prototype_es6.js
```Javascript
class Enemy {
    constructor(life, armor) {
        this.life = life;
        this.armor = armor;
    }

    attacked(pwr) {
      this.life -= pwr;
    }

    clone() {
        return new Enemy(this.life, this.armor);
    }
}

export default Enemy;

```

### Singleton
##### singleton_es6.js
```Javascript
class DbConnection {
    constructor() {
        if (typeof DbConnection.instance === 'object') {
            return DbConnection.instance;
        }
        this.Connstring = "connection1";
        this.connected = false;
        DbConnection.instance = this;
        return this;
    }

    getStatus() {
      return this.connected;
    }

    connect() {
      this.connected = true;
    }

    changeConnection(connString) {
      this.Connstring = connString;
    }

    getConnection() {
      return this.Connstring;
    }

}

export default DbConnection;

```


## structural
### Adapter
##### adapter_es6.js
```Javascript
class RoundHole {
  constructor(radius) {
    this.radius = radius;
  }

  fits(object) {
    if (!(object instanceof RoundObject))
        throw "Not a round object";
    return this.radius >= object.getRadius();
  }

}

class RoundObject {
  constructor(radius) {
    this.radius = radius;
  }

  getRadius() {
    return this.radius;
  }
}

class SquareObject {
  constructor(width) {
    this.width = width;
  }

  getWidth() {
    return this.width;
  }
}

class RoundSquareAdapter extends RoundObject{
  constructor(square) {
    super();
    if (!(square instanceof SquareObject))
        throw "Not a square object";
    this.square = square;
  }

  getRadius() {
    return Math.sqrt(2 * Math.pow(this.square.getWidth(), 2)) / 2;
  }

}


export { RoundSquareAdapter, SquareObject, RoundObject, RoundHole };

```

### Decorator
##### decorator_es6.js
```Javascript
function replaceAll(str, find, replace) {
    return str.replace(new RegExp(find, 'g'), replace);
}

class DataSource {
  constructor() {
    this.data = null;
  }

  setData(data) {
    this.data = data;
  }

  readData() {
    console.log('Raw Data', this.data);
    return this.data;
  }

}

class DataSourceDecorator extends DataSource {
  constructor(dataSource) {
    super();
    this.dataSource = dataSource;
  }

  setData(data) {
    this.dataSource.setData(data);
  }

  readData() {
    const data = this.dataSource.readData();
    return data;
  }
}

class EncryptData extends DataSourceDecorator {
  constructor(dataSource) {
    super(dataSource);
  }

  setData(data) {
    const encryptedData = `XXX===${data}===XXX`;
    super.setData(encryptedData);
  }

  readData() {
    let encryptedData = super.readData();
    const decryptedData = replaceAll(replaceAll(encryptedData, 'XXX===',''),'===XXX','');
    return decryptedData;
  }
}

class CompressData extends DataSourceDecorator {
  constructor(dataSource) {
    super(dataSource);
  }

  setData(data) {
    const compressData = replaceAll(data, 'XXX', 'x');
    super.setData(compressData);
  }

  readData() {
    let compressData = super.readData();
    const decompressedData = replaceAll(compressData, 'x','XXX');
    return decompressedData;
  }
}

export { DataSource, DataSourceDecorator, EncryptData, CompressData };

```

### Facade
##### facade_es6.js
```Javascript
class MemoryRepo {
  constructor() {
    this.internalData = new InMemory();
  }

  getObject(id) {
    const obj = this.internalData.getObject(id);
    if (obj === undefined)
      return null;
    return obj;
  }

  addOrUpdateObject(object) {
    this.internalData.addOrUpdateObj(object);
  }

}

class MemoryDbRepo {
  constructor() {
    this.dbData = new DbMemory();
  }

  getObject(id) {
    try {
      return this.dbData.queryById(id);
    }
    catch (e) {
      if (e == "Not Connected")
         this.dbData.connect();
      try {
        return this.dbData.queryById(id);
      }
      catch (e) {
        return null;
      }
    }
  }

  addOrUpdateObject(object) {
    try {
      this.dbData.insertToDb(object);
    }
    catch (e) {
      if (e == "Not Connected")
         this.dbData.connect();
      try {
        this.dbData.insertToDb(object);
      }
      catch (e) {
        this.dbData.updateObject(object);
      }
    }
  }
}

class InMemory {
  constructor() {
    this.data = {};
  }

  addOrUpdateObj(object) {
    this.data[object.id] = object;
  }

  getObject(id) {
    return this.data[id];
  }

}

class DbMemory {
  constructor() {
    this.connected = false;
    this.db = {};
  }

  connect() {
    this.connected = true;
  }

  queryById(id) {
    if (!this.connected)
      throw "Not Connected";
    if (this.db[id] === undefined)
      throw "Not found";
    return this.db[id];
  }

  insertToDb(object) {
    if (!this.connected)
      throw "Not Connected";
    if (this.db[object.id] !== undefined)
      throw "Duplicate";
    this.db[object.id] = object;
  }

  updateObject(object) {
    if (!this.connected)
      throw "Not Connected";
    if (this.db[object.id] === undefined)
      throw "Not found";
    this.db[object.id] = object;
  }

}

class DataObject {
  constructor(id, name) {
    this.id = id;
    this.name = name;
  }
}

export { MemoryRepo, MemoryDbRepo, DbMemory, InMemory, DataObject };

```

### Flyweight
##### flyweight_es6.js
```Javascript
class TreeType {
    constructor(type, color, texture){
      this.type = type;
      this.color = color;
      this.texture = texture;
    }
}

class ShallowTree {
    constructor(type, height){
      this.type = type;
      this.height = height;
    }
    render(treeType) {
      console.log(`${this.height} - ${treeType.color} - ${treeType.texture}`);
    }
    getSize() {
      return roughSizeOfObject(this);
    }
}

class DeepTree {
    constructor(treeType, height){
      this.treeType = treeType;
      this.height = height;
    }
    render() {
      console.log(`${this.height} - ${this.treeType.color} - ${this.treeType.texture}`);
    }
    getSize() {
      return roughSizeOfObject(this);
    }
}

class ShallowforestFactory {
    constructor(){
        this.treeTypes = {};
        this.trees = [];
    }
    addTrees(treeType, count) {
        if (this.treeTypes[treeType.type] === undefined)
          this.treeTypes[treeType.type] = treeType;
        for (let i = 0; i <= count; i++)
          this.trees.push(new ShallowTree(treeType.type, i * 10));
    }
    renderForest() {
      for (let i = 0; i < this.trees.length; i++)
        this.trees[i].render(this.treeTypes[this.trees[i].type]);
    }
    getSize() {
      let size = 0;
      for (let i = 0; i < this.trees.length; i++)
        size += this.trees[i].getSize();
      return size;
    }
};

class DeepforestFactory {
    constructor(){
      this.trees = [];
    }
    addTrees(treeType, count) {
      for (let i = 0; i <= count; i++)
        this.trees.push(new DeepTree(treeType, i * 10));
    }
    renderForest() {
      for (let i = 0; i < this.trees.length; i++)
        this.trees[i].render();
    }
    getSize() {
      let size = 0;
      for (let i = 0; i < this.trees.length; i++)
        size += this.trees[i].getSize();
      return size;
    }
};

function roughSizeOfObject( object ) {

    var objectList = [];
    var stack = [ object ];
    var bytes = 0;

    while ( stack.length ) {
        var value = stack.pop();

        if ( typeof value === 'boolean' ) {
            bytes += 4;
        }
        else if ( typeof value === 'string' ) {
            bytes += value.length * 2;
        }
        else if ( typeof value === 'number' ) {
            bytes += 8;
        }
        else if
        (
            typeof value === 'object'
            && objectList.indexOf( value ) === -1
        )
        {
            objectList.push( value );

            for( var i in value ) {
                stack.push( value[ i ] );
            }
        }
    }
    return bytes;
}


export { TreeType, ShallowTree, DeepTree, ShallowforestFactory, DeepforestFactory };

```

### Proxy
##### proxy_es6.js
```Javascript
class Car {
    drive() {
        return "driving";
    };
}

class CarProxy {
    constructor(driver) {
        this.driver = driver;
    }
    drive() {
        return (this.driver.age < 18) ? "too young to drive" : new Car().drive();
    };
}

class Driver {
    constructor(age) {
        this.age = age;
    }
}

export { Car, CarProxy, Driver };

```



