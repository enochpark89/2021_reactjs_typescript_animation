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
- import framer-motion to start.
- You cannot animate <div></div>. Anything you animation has to come out from motion.

```js
import styled from "styled-components";
import {motion} from "framer-motion";

<div></div>
<motion.div><motion.div>
```

## EcmaScript Error

*Error message: Can't import the named export 'Children' from non EcmaScript module (only default export is available*

- This error happends because the motion has been converted to EcmaScript module and create-react-app version4 doesn't accept EcmaScript yet. 

- We have to override configuraiton of create-react-app
- We can use *CRACO(Create React App Configuration Override)*
    - URL: github.com/gsoft-inc/craco

## Installing CRACO

1. Use the bash script
```js
npm install @craco/craco --save
```
2. Create a config file in the root.
*Please refer to the documentation*
```js
touch craco.config.js
```

3. Copy paste to fix the bug on the issue below

url: https://github.com/framer/motion/issues/1307#issuecomment-955877279

craco.config.js:
```js
module.exports = {
    webpack: {
          configure: {
              module: {
                  rules: [
                      {
                          type: 'javascript/auto',
                          test: /\.mjs$/,
                          include: /node_modules/,
                      },
                  ],
              },
          },
      },
  }
```

4. Replace the script to below:

url: https://github.com/gsoft-inc/craco/blob/master/packages/craco/README.md#installation

```js
/* package.json */

"scripts": {
-   "start": "react-scripts start",
+   "start": "craco start",
-   "build": "react-scripts build",
+   "build": "craco build"
-   "test": "react-scripts test",
+   "test": "craco test"
}
```

5. Validate the EcmaScript Error fix.

# 2. Basic Animation

- If you want to animate something, you have to bring that animation to motion. 

*How do we have motion animated styled-components?*

```js
// step 1: set motion on the div when you create a styled components.
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: white;
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

// step 2: use the styled component and add props like transition, initial, and animate to create an animation.
function App() {
  return (
    <Wrapper>
      <Box
        transition={{ type: "spring", delay: 0.5 }}
        // start with 0: disappeared state.
        initial={{ scale: 0 }}
        // ends with 1: it appears.
        animate={{ scale: 1, rotateZ: 360 }}
      />
    </Wrapper>
  );
}

```
- initial: you can set the initial state of the animation
- All animation bonces because all animation uses spring().
- If you want to get rid of the bounding, you can change by using a different type of a transition like twin(), which is very linear. 
- transition {{ex: damping, stiffness}}
*Please refer to the documentation to check all the props available.*
- damping 0 will cause component to oscillate forever. 

# 3. Variants

- Variants help clean up your code. 
- The component code can become cleaner. 

```js
// myVars object is created.
const myVars = {
  start: { scale: 0 },
  end: { scale: 1, rotateZ: 360, transition: { type: "spring", delay: 0.5 } },
};

// You can send the object using Box.varients prop.
function App() {
  return (
    <Wrapper>
      <Box variants={myVars} initial="start" animate="end" />
    </Wrapper>
  );
}

```

*Please note that initial="start" and animate="end" need to be set.*

**Create a varient v.2**

1. Create a new Circle styled component
```js
const Circle = styled(motion.div)`
  background-color: white;
  height: 70px;
  width: 70px;
  place-self: center;
  border-radius: 35px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;
```

2. Create a boxVarient object for a varient.

```js
const boxVariants = {
  start: {
    opacity: 0,
    scale: 0.5,
  },
  end: {
    scale: 1,
    opacity: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
      delayChildren: 0.5,
      staggerChildren: 0.2,
    },
  },
};
```

3. Create a parent and children components

```js
// By deafult, if a parent has initial and animate, it will be inherited to children.
      <Box variants={boxVariants} initial="start" animate="end">
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
```

4. When you are creating a childVarient object, follow the same names as a parentVarient.

```js
const circleVariants = {
  start: {
    opacity: 0,
    y: 10,
  },
  end: {
    opacity: 1,
    y: 0,
  },
};
```
*How do I make the each circle appear one at a time?*

- Motion has a utility that can accomplish this. 
    - delay
    - dealyChildren
- *DelayChildren* can delay the children component from appearing at a rate of speed you want to appear.
- *StaggerChildren* means you you want to create a delay on each components so that each component will appear one after the other. 
- x: and y: are very specific to Motion but others exist in CSS, which makes it convenient.

Final - App.tsx
```js
import styled from "styled-components";
import { motion } from "framer-motion";
const Wrapper = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: white;
  height: 70px;
  width: 70px;
  place-self: center;
  border-radius: 35px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  start: {
    opacity: 0,
    scale: 0.5,
  },
  end: {
    scale: 1,
    opacity: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
      delayChildren: 0.5,
      staggerChildren: 0.2,
    },
  },
};

const circleVariants = {
  start: {
    opacity: 0,
    y: 10,
  },
  end: {
    opacity: 1,
    y: 0,
  },
};

function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial="start" animate="end">
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
export default App;

```

# 4. Gestures

- you can use *whileHoever* prop in the box. 
- you can do *whileTap* prop - clickw will initiate the aniimation. 
- you can also use variants to make it easier. 
- you can create a dragging with *whileDrag*
- 

```js
      <Box
        drag
        variants={boxVariants}
        whileHover="hover"
        whileDrag="drag"
        whileTap="click"
      />
```

```js
const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46, 204, 113)", transition: { duration: 10 } },
};

function App() {
  return (
    <Wrapper>
      <Box
        drag
        variants={boxVariants}
        whileHover="hover"
        whileDrag="drag"
        whileTap="click"
      />
    </Wrapper>
  );
```

- There are drags constraints. We can put a constraints.
  1. lock the scroll to one direction. (only x) 
  2. contraint on how far you can move. 
  3. There is a force that could bring things to the center. 

- You can create a reference and assign that to the prop for use. This is where the usefulness of useRef() comes in. 

# MotionValue

- MotionValue track the state and velocity of animating values. 
- dragSnapToOrigin: come back to the center. 
- If you want to keep track of the value of x or y, 
- even if the component changes, it will not rerender.
- Motion is keeping track of motionValue but it does not keep track witin ReactJS. 
```js
// the value is being tracked. 
const x = useMotionValue(0); 
  return (
    <Wrapper>
      <Box style={{ x }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```
- useTransform(): You are setting up different scenarios. 
- when x is -800, you want to get 2, when 0 , you want to get 0 . 
- We are basically mapping the value so that the x value will change as they reach certain point.

```js
import { motion, useMotionValue, useTransform } from "framer-motion";
function App() {
  const x = useMotionValue(0);
  const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);
  return (
    <Wrapper>
      <Box style={{ x, scale }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```

- Therefore, you can modify the scale. 

*What can you do more with userTransform?*

1. RotateZ
2. Change background depending on the location of a component gradually using gradient. 
```js
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  const { scrollYProgress } = useViewportScroll();
  const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);
```