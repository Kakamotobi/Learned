# Getters and Setters

## Getter
- Use to access properties in an object.
- Read method like a property.
- Prefix the method with the **`get`** keyword.
  - Now, we can access the method like a property (Ex: `person.fullName`).

## Setter
- Use to change or mutate properties in an object from the "outside".
- Prefix the method with the **`set`** keyword.
- Needs a parameter `value`.
  - `value` will be what we have on the right side of the assignment operator.

## Example
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

