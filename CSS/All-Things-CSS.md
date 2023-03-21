# All Things CSS

## Table of Contents
- [Global CSS](#global-css)
- [CSS Processors](#css-processors)
  - [Preprocessors](#preprocessors)
  - [Postprocessors](#postprocessors)
  - [Preprocessor vs Postprocessor](#preprocessor-vs-postprocessor)
- [CSS-in-CSS](#css-in-css)
- [CSS-in-JS Libraries](#css-in-js-libraries)
- [Utility Class Libraries/Frameworks](#utility-class-libariesframeworks)
- [Component Library (for React)](#component-library-for-react)

## Global CSS
- CSS files in the global scope.

## CSS Processors
### Preprocessors
- Ex: SASS, LESS.
- Pre-processing has its own stylesheet language that allow the use of variables, loops, functions, and mixins within CSS.
	- i.e. basic CSS + extended functionality.
- These preprocessing language converts into pure CSS.
### Postprocessors
- Ex: PostCSS
- Postprocessors operate “post” development to extend it further through automation.
- i.e. after the plain CSS has been produced, post processors apply automation/repetition.
- For example, class selectors can be extended, and prefixes (Ex: -webkit, -moz) are auto-appended for certain CSS properties.
- PostCSS uses JS plugins to automate your CSS workflow.
- PostCSS Plugins
	- https://github.com/postcss/postcss/blob/main/docs/plugins.md
### Preprocessor vs Postprocessor
- Sass can also auto-append prefixes by using extensions.
- PostCSS can actually work in both preprocessing and postprocessing.
  
## CSS-in-CSS
- This includes CSS Modules, processors like SASS, etc.
- This approach is faster than CSS-in-JS because it doesn’t have to interpret JS. Therefore, it is usually better for projects requiring lots of interaction and rendering.

## CSS-in-JS Libraries
- JSS
- Styled JSX
	-   https://reactresources.com/topics/styled-jsx
- This approach is slower than CSS-in-CSS. However, this approach may be better if development efficiency is important, since it only loads the CSS styles pertaining to the component page.

## Utility Class Libraries/Frameworks
- TailwindCSS
	- Provides utility classes that help us quickly style our components for a good looking UI, instead of writing the CSS directly.
	- Requires some tooling and configuration to get started.
	- Quicker because it supports IDE intellisense to pick styles quickly.
	- It purges your unused CSS automatically for a very efficient bundle size.
	- However, use of utility classes can result to complex HTML. Therefore, organizing your code is important.
	- TailwindCSS doesn’t provide prebuilt components for you. Therefore you still need to do a lot of work on your own.
- Bootstrap, Bulma
	- Provides utility classes and prebuilt components as well.
	- Could lead to a larger bundle size because unused classes could be included in the final CSS.

## Component Library (for React)
- Component libraries provide a more tightly integrated approach and are usually more opinionated.
- React Bootstrap
- Mantine
	- Provides a lot of utilities (that you would otherwise have to do in JS) such as hooks to use the intersection observer API to know when an element is visible, modals, notifications, calendars, etc.
- Material UI
- Chakra
- Ant Design

## Resources
[CSS Post-Processors For Beginners: Tips and Resources - Hongkiat](https://www.hongkiat.com/blog/css-post-processors-tips-resources/)  
[7 ways to deal with CSS - YouTube](https://www.youtube.com/watch?v=ouncVBiye_M&ab_channel=Fireship)  
[PostCSS in 100 Seconds - YouTube](https://www.youtube.com/watch?v=WhCXiEwdU1A&ab_channel=Fireship)  
[How to Use PostCSS as a Configurable Alternative to Sass — SitePoint](https://www.sitepoint.com/postcss-sass-configurable-alternative/)  
[A Unified Styling Language. In the past few years we’ve seen the… | by Mark Dalgleish | SEEK blog | Medium](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660)  
[웹 컴포넌트 스타일링 관리 CSS-in-JS vs CSS-in-CSS | 인사이트리포트 | 삼성SDS](https://www.samsungsds.com/kr/insights/web_component.html)  
