# Animation

- Animation with Reactjs.

# Introduction
- Build anymation using Framer Motion
- Framer Motion is a library that you can build. 
- Framer is the company that builds for designers. Prototype things. 
- There are various effects that come with it.
- When you are building an animation for companies, this would be a good tool to use. 

# 1. Installation

*Please refer to the link: github.com/framer/motion*

- Start by reading a quickstart.
```js
npm install framer-motion
```
- Quickstart guide and uses. 
```js
import {motion} from "framer-motion"

export const MyComponent = ({ isVisible}) => (
    <motion.div animate={{ opacity: isVisible ? 1 : 0}} />

)
```