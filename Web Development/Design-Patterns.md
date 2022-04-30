# Design Patterns

## Table of Contents
- [Model-View-Controller (MVC)](#mode-view-controller-mvc)

## Model-View-Controller (MVC)
> MVC is an architectural design pattern that encourages improved application organization through a separation of concerns. It enforces the isolation of business data (Models) from user interfaces (Views), with a third component (Controllers) traditionally managing logic and user-input. | patterns.dev
- MVC is a design pattern that we as developers can adopt/apply.
  - Angular.js is an MVC framework.
  - React is not an MVC framework. It is a View library. But we can use the MVC pattern for a React component.
### Brief History
- Smalltalk-80 MVC was designed in 1979.
- It aimed to separate out the application logic from the UI.
  - Idea: decoupling the application logic and the UI would allow the reuse of models for other interfaces in the application.
- MVC
  - A Model represented domain-specific data that was separate from the UI (Views and Controllers).
  - A View represented the current state of a Model. Whenever changes were made to the Model, the Observer pattern was used to inform the View.
    - A View-Controller pair was required for each element displayed on the screen.
      - The View took care of presentation.
      - The Controller handled user interactions (Ex: clicks, key-presses).
- Key point: the View observes the Model. So when the Model changes, the View react.
### JavaScript MVC
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/MVC.png" alt="MVC Illustration" width="80%" /><br>
  <em>The definition and role of each can differ based on the project/implementation.</em>
</p>

#### Models
- **Manage the data/state for an application.**
- They are not concerned with the UI or presentation layers.
- They simply represent data and logic related to data that an application may require.
- When the Model changes (Ex: updated data), its observers (views) are notified of it.
- Model persistence is achieved by saving its most recent state in memory, localStorage, or synchronized with a database.
- A Model may have multiple views observing it.
#### Views
- **Visual representations of models that present a filtered view of their current state.**
- Concerned with building and maintaining a DOM element.
- A View observes its Model and updates itself accordingly whenever the Model changes.
- Views are simply "dumb" user interface that provides user interaction (Ex: get, set values in Models).
  - They are not responsible for any actions.
  - *Controllers are the ones responsible for actually updating Models.*
#### Controllers
- **The intermediary between Models and Views that are responsible for updating the Model upon user interaction/manipulation of the View.**
- Handles business logic and events (usually triggered by the user).
### Reference
[Learn JavaScript Design Patterns - patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/)  
[Elements of MVC in React. Let’s discover the original MVC pattern… | by Daniel Dughila | The Startup | Medium](https://medium.com/swlh/elements-of-mvc-in-react-9382de427c09#:~:text=React%20isn't%20an%20MVC,nothing%20to%20do%20with%20frameworks.)  
[How JavaScript works: modularity and reusability with MVC | by Ukpaiugochi | SessionStack Blog](https://blog.sessionstack.com/how-javascript-works-writing-modular-and-reusable-code-with-mvc-16c65cbd9f64)  

