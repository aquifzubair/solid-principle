# Report to change the code base into SOLID Principle for better code understanding.

### What is SOLID?
SOLID is the group of five principles, Which is used to make code base more readable and maintainable. This principle was introduced by **Robert C. Martin**. There is a meaning of every letter present in **SOLID**, and they all are the principles of **SOLID**.

SOLID stands for:-
* **S** :- Single Responsibility Principle
* **O** :- Open Closed Principle
* **L** :- Liskov Substitution Principle
* **I** :- Interface Segregation Principle
* **D** :- Dependency Inversion Principle

## Single Responsibility Principle :-
As the name suggest single, It means a class should have only one task. Let me clear this with an example.

```
// Bad Way

class traineeLeave {
    constructor(trainee){
        this.trainee = trainee;
    }
    
    wantLeave(trainee){
        leaveTracker() ? true : false;
    }

    leaveTracker(trainee){
        if(numOfRemainingLeaves > 0){
            return true;
        }
        else return false;
    }

}
```
```
// Good Way

class trackLeave {
    constructor(trainee){
        this.trainee = trainee;
    }

    leaveTracker(trainee) {
         if(numOfRemainingLeaves > 0){
            return true;
        }
        else return false;
    }
}

class traineeLeave {
    constructor(trainee){
        this.trainee = trainee;
        this.tracker = new trackLeave(trainee) 
    }
    
    wantLeave(trainee){
        this.tracker.leaveTracker() ? true : false;
    }
}
```

In both examples above, first example is bad in the way that we are writing `leaveTracker` and` wantLeave` method in single class which doesn't follow the **Single Responsibility Principle**. In second example we just created an another class to track the leave and use the method of this class in want leave class. Now we can see that every class has its own single task. Which is the best practice to achieve **Single Responsibility Principle**.

## Open Closed Principle :- 

**Objects should be open for extensions and closed for modification.** 

open for extension means that we should be able to add new features without breaking the code and closed for modification means that class has been well defined and changes in this class will lead to the change in large area of code base. 

Example :- 
```
// Bad way
class checkPlacedTrainee {
    constructor(trainee, listOfPlacedStudent){
        this.trainee = trainee;
        this.listOfPlacedStudent = listOfPlacedStudent;
    }
    placementChecker(trainee){
        if(trainee in the list of placed student){
            return true;
        }
        else
            return false;
    }
}


class deployedTrainee {
    constructor(trainee, listOfTrainees){
        this.listOfTrainees = listOfTrainees;
        this.trainee = trainee;
        this.traineeCheker = new CheckPlacedTrainee(trainee)
    }
    listOfTrainees = ['aquif', 'sameer', 'pallabi', 'kalaiselvi'];
    checkTrainee(){
        if(this.traineeChecker.placementChecker(trainee)){
            console.log(`congrats ${trainee} is placed);
        }
        else {
            console.log(`${trainee} is yet to be placed`)
        }
    }
    
```
```
// Good way

class checkPlacedTrainee {
    constructor(trainee, listOfPlacedStudent){
        this.trainee = trainee;
        this.listOfPlacedStudent = listOfPlacedStudent;
    }
    placementChecker(trainee){
        if(trainee in the list of placed student){
            return true;
        }
        else
            return false;
    }
}


class deployedTrainee {
    constructor(trainee, listOfTrainees){
        this.listOfTrainees = listOfTrainees;
        this.trainee = trainee;
        this.traineeCheker = new CheckPlacedTrainee(trainee)
    }
    listOfTrainees = ['aquif', 'sameer', 'pallabi', 'kalaiselvi'];
    checkTrainee(){
        if(this.traineeChecker.placementChecker(trainee)){
            console.log(`congrats ${trainee} is placed);
        }
        else {
            console.log(`${trainee} is yet to be placed`)
        }
    }
    addTrainee(trainee) {
        this.listOfTrainees.push(trainee);
    }
}
```
In the first example when we have to add trainee we don't have any method to add we have to change the array of list of trainees that's why this is a bad example because when we are trying to extend the feature we are breaking the existing code. In the second example we just add an `addTrainee` method then we don't have to change the list of trainees manually we can just push by `addTrainee` method.

## Liskov Substitution Principle :- 
If you have a parent class and a child class then we can use parent class and child class interchangeably without getting the wrong results. Let's understand with an example.

```
// Bad Way, will get wrong results.
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea();
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```
```
// Good way
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}
```
In first example square class is extended from a rectangle class but square class some times would not give us a desired result. When we used 4,5 as parameters of rectangle and use it with a square class it gives us the area of rectangle but we were suppossed to get the area of square. In second example rectangle and square class are the child class of shape class. In this scenario we can see o/p will be never wrong either it is rectangle or square.


## Interface Segregation Principle :-
Javascript doesn't have interface so this principle doesn't apply strictly here. So we can't avoid this principle in our project.

## Dependency Inversion Principle :-
In this principle high level modules shouldn't depends upon low level modules.

```class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```
```
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

In first example we are just passing item in inventory tracker which will always request http but in second example we have two parameters in inventory tracker which is item in requester. We can pass any requester we want.


**Note :- Some of the examples are taken from this [link](https://www.youtube.com/watch?v=XzdhzyAukMM)**







