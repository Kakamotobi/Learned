# Getters and Setters
- Accessor functions.
- Used to define methods on a class, which are then used as if they are properties.
  - i.e., they look like properties but are actually methods of the class.

## Accessor Property
> 접근자란 객체 지향 프로그래밍에서 객체가 가진 프로퍼티 값을 객체 바깥에서 읽거나 쓸 수 있도록 제공하는 메서드를 말합니다. 객체의 프로퍼티를 객체 바깥에서 직접 조작하는 행위는 데이터의 유지 보수성을 해치는 주요한 원인입니다. - 아소 히로시, 모던 자바스크립트 입문 9.3

## Getters
- Use to access properties in an object.
- Prefix the method that will serve as the getter method with the **`get`** keyword.
  - Now, we can access the method like a property (Ex: `person.fullName`).

## Setters
- Use to change or mutate properties in an object.
- Prefix the method that will serve as the setter method with the **`set`** keyword.
- Needs a parameter `value`.
  - `value` will be what we have on the right side of the assignment operator.
- You can assign new values to the properties of an instance with a property-like syntax.

## Example 1
```
const person = {
  firstName: "Tom",
  lastName: "Jerry"
  get fullName() {
    return `${person.firstName} ${person.lastName}`
  }
  set fullName(value) {
    // split the string by a space and take the parts and set the firstName and lastName properties.
    const parts = value.split(" ");
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
};

// setter that will change the initial name to a new value.
person.fullName = "Johnny Bravo"
```
- *Through the getter and setter methods, a virtual property called fullName is created. Virtual properties can be read and used but don't actually exist.*

## Example 2
```
class Square {
  constructor(_width) {
    this.width = _width;
    this.height = _width;
    this.numOfRequestsForArea = 0;
  }
  
  get area () {
    this.numOfRequestsForArea++;
    return this.width * this.height;
  }
  
  set area (area) {
    this.width = Math.sqrt(area);
    this.height = this.width;
  }
}

let square1 = new Square(4);
console.log(square1.area) // the method "area" behaves like a property (no need for ()).

square1.area = 25; // The setter converts the 25 into the width and height.
console.log(square1.area); // Now 25.
console.log(square1.width, square1.height); // Converted the initial width/height of 4 to 5 using the setter, which modified the width/height based on the area that has been passed in.

console.log(square1.numOfRequestsForArea);
```

## What's the Point of Getters and Setters?
### The Getter and Setter functions are neater and clearer in syntax
- Getters get something.
- Setters set something.
- Methods run.
#### Example
```
const carousel = {
  // Option 1: Normal Function
  getCurrentSlide() {}

 // Option 2: Getter and Setter Functions
  get currentSlide() {}
  set currentSlide(value) {}
}
```
```
// Getter

// Normal Function
const currentSlide = carousel.getCurrentslide();

// Getter Function
const currentSlide = carousel.currentSlide;
```
```
// Setter

// Normal Function
carousel.setCurrentSlide(4);

// Setter Function
carousel.currentSlide = 4;
```

### Encapsulation
> Encapsulation is the packing of data and functions into one component (for example, a class) and then controlling access to that component to make a "blackbox" out of the object. Because of this, a user of that class only needs to know its interface (that is, the data and functions exposed outside the class), not the hidden implementation.
- Setting up of the data-getters-setters trio to access and modify that data. Useful when some operations (ex: validations) have to be performed on the data before saving or viewing it.
- To hide data, make it inaccessible from other code, except through the getters and setters. This way, we don't end up accidentally overwriting important data with some other code in the program.

## Reference
[JavaScript - 접근자 프로퍼티 (getter, setter)](https://velog.io/@bigbrothershin/JavaScript-%EC%A0%91%EA%B7%BC%EC%9E%90-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-getter-setter)  
[JavaScript Class #2: Getters & Setters - JavaScript OOP Tutorial - YouTube](https://www.youtube.com/watch?v=y4wDanUBNmE&ab_channel=dcode)  
[Encapsulation | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Encapsulation)
