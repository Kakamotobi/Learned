# CSS Animation

## `@keyframes`
- The various points in time where the intermediate steps along the animation sequence are defined.
- Syntax:
```
@keyframes slidein {
  from {
    transform: translateX(0%);
  }
  to {
    transform: translateX(100%);
  }
}
```
### Example
```
#hexagon {
  // @keyframes name | duration | iteration count
  animation: bobble 2s infinite;
}

@keyframes bobble {
  // active at 0s
  0% {
      transform: translateY(10px);
  }
  // active at 1s
  50% {
      transform: translateY(40px);
  }
  // active at 2s
  100% {
      transform: translateY(10px);
  }
}
```
- Percentage value represents the percentage of time through the animation sequence at which the specified keyframe becomes active.
