# My React Understandings

### Example code

```javascript
import React from "react"
import ReactDOM from "react-dom/client"

const firstElement = React.createElement('h1',{
    id:'someIdOne'
},"First Line")

const secondElement = React.createElement('h1',{
    id:'someIdTwp'
},"Second Line")

const divElement = React.createElement('div',{
    id:'divId'
},[firstElement,secondElement])

React.CreateElements => Object => HTML(DOM)

const rootReactEle = ReactDOM.createRoot(document.getElementById('root'))

rootReactEle.render(divElement)
```

This is how basic react code looks like under the hood

## PACKAGE MANAGER

**npx** - executing commands without downloading packages.  
**npm** - will download required packages.

**package.json** is a file with all the information related to packages in our project.  
**package-lock.json** files tells us which version of package is running in our application and locks the versions.

## ALL_ABOUT_BUNDLER

(We can build react without create-react-app, just can setup our own bundler and install react)  
**Some Bundlers** _webpack, vite, parcel_

We need bundler to bundle all react code to make it production react ,using parcel to bundle in this example.
bundler is responsible but compression, minifications , bundleing,hot reloading,image optimiziation , tree shaking(removing unwanted code ex: if we import a library / package which has lot of helper functions which are not needed by current project so it removes unwanted code ) and lot more stuff.

## JSX

### Example code

```javascript
React.createElement => Object => HTML(DOM) (Process of converting React code to HTML to render)

const firstElement = React.createElement('h1',{
    id:'someIdOne'
},"First Line")

const secondElement = React.createElement('h1',{
    id:'someIdTwo'
},"Second Line")

const divElement = React.createElement('div',{
    id:'divId'
},[firstElement,secondElement])
```

writing everything with React.createElement is messy and lengthy and unreadable. So comes the concept of JSX (FB developers built JSX).

```javascript
const jsxCodeOfAbove = (
  <div>
    <h1 id="someIdOne">First Line</h1>
    <h1 id="someIdTwo">Second Line</h1>
  </div>
);
```

It is html like syntax but not HTML inside js, This code cannot be complied by browser directly. There comes some Babel

**_JSX =>(Babel Catalyst)=> React.createElement => Object => HTML(DOM)_**

## BABEL

just a js package. It converts the modern js code in older js code (process is known as Polyfill,Babel writes polyfills to support old browsers).
Use babel-plugin-transform-remove-console to remove console.logs.

Babel converts JSX code into Abstract Syntax Tree then JS engine will convert into bute code some cool stuff happens

## React Components:

EveryThing in React is a component
React Components are two type
Functional Component - NEW way of writing (simply js function)
Class Component - OLD and classic

## Functional Component

Functional components are just js functions at the end of the day
returns some JSX

```javascript
const FunctionalExample = () => (
  <div>
    <h1> Returns some JSX</h1>
  </div>
);
```

## Class Components

Class Components is normal js class
render() method is mandatory method in class component
render returns some JSX

### Basic example

```javascript
class BasicClass extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <div>
        <h1></h1>
      </div>
    );
  }
}
```

### Code example of React element

This is Element

```javascript
const jsxCodeOfAbove = (
  <div>
    <h1 id="someIdOne">First Line</h1>
    <h1 id="someIdTwo">Second Line</h1>
  </div>
);
```

Good practice is to write components with Pascal case it is not mandatory allthrough your code still works

### Diff b/n React Element & React Component:

React Element is returning an Object.
React Component is a function that returns JSX, or a react element, or a function.

Syntax When rendering:
For React Element, We use `root.render(element_name);`
For React Component, We use Angular brackets:
`root.render(<ComponentName />);`

Any piece of Javascript code can be written within {}

XSS - Cross site scripting (XSS) is an attack in which an attacker injects malicious executable scripts into the code of a trusted application or website. Attackers often initiate an XSS attack by sending a malicious link to a user and enticing the user to click it.

JSX takes care of XSS.

#### XSS code example

```javascript
const JsxCodeOfAbove =()=> (
    <div>
        {some js code}
        <h1 id="someIdOne" >First Line</h1>
         <h1 id="someIdTwo" >Second Line</h1>
    </div>
)
const rootReactEle = ReactDOM.createRoot(document.getElementById('root'))

rootReactEle.render(JsxCodeOfAbove)
```

## COMPONENT_LIFE_CYCLE

Component life cycle has 3 phases

> ### Mounting => Updating => Unmounting
>
> React will do these in 2 phases
>
> ### Render phase and Commit phase
>
> Refer the link which illurates the component life cycle diagrams  
> [Component life cycle diagram reference link react](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  
> Lets first understand life cycle methods using class components

### Examples to understand what are called first

> **Constructor => render => componentDidMount => render(if anything changed) => componentDidUpdate => componentWillUnmount**

The order component cycle will call

> parent-constructor => parent-render => here react returns JSX which contains another component so react completes that component cycle so => child-constructor=>

    child-render => child-did-mount => parent-did-mount

```javascript
class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
    console.log("parent-constructor");
  }
  componentDidMount() {
    console.log("parent-did-mount");
  }
  render() {
    console.log("parent-render");
    return (
      <div>
        <h1></h1>
        <Child />
      </div>
    );
  }
}

class Child extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
    console.log("child-constructor");
  }
  componentDidMount() {
    console.log("child-did-mount");
  }
  render() {
    console.log("child-render");
    return (
      <div>
        <h1></h1>
      </div>
    );
  }
}
```

Lets look at if there are 2 child components
Order of react components call

> parent-constructor =>parent-render => child-constructor-1 => child-render-1 => child-constructor-2 => child-render-2=>child-diMount-2=>child-diMount-1=>parent-diMount-1

```javascript
class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
    console.log("parent-constructor");
  }
  componentDidMount() {
    console.log("parent-did-mount");
  }
  render() {
    console.log("parent-render");
    return (
      <div>
        <h1></h1>
        <Child num={1} />
        <Child num={2} />
      </div>
    );
  }
}

class Child extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
    console.log("child-constructor" + this.props.num);
  }
  componentDidMount() {
    console.log("child-did-mount" + this.props.num);
  }
  render() {
    console.log("child-render" + this.props.num);
    return (
      <div>
        <h1></h1>
      </div>
    );
  }
}
```

## SOME IMPORTANT THINGS TO UNDERSTAND IN COMPONENT LIFE CYCLE

This below is component life cycle in class lets check with both class and functional components

```javascript
class Child extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
        console.log('first called and intilization happen')
    }
    componentDidMount(prevProps,prevState){
        if(this.state.someting !=== prevState.something){
            // Do someting this is equvalent to useEffect with dependency see below functional component example
        }
        if(this.state.someting2 !=== prevState.something2){
            // Do someting
        }
        console.log('after first render it calls')
    }
    componentDidUpdate(){
        console.log('after after every re-render')
    }
    componentWillUnmount(){
        console.log('called before component is leaving')
    }
    render(){
        console.log('called after intilization and also after every re-render')
        return(
            <div>
                <h1></h1>
            </div>
        )
    }
}
```

> _**useEffect is not equvalent to componentDidMount never compare functional components with component life cycles
> we can do every phase using useEffect see example**_

```javascript
const MyComponent = () => {
  const [something] = useState(""); // initilizing state

  useEffect(() => {
    // This will call after every re-render like componentDidUpdate
  });
  useEffect(() => {
    // after first render it calls like componentDidMount
    return () => {
      // called before component is leaving like componentWillUnmount
    };
  }, []);

  useEffect(() => {
    // This is equal to compare state with prevState kind of thing in componentDidUpdate
  }, [something]);

  useEffect(() => {
    // This is equal to compare state with prevState kind of thing in componentDidUpdate
  }, [something2]);

  return (
    <div>
      <h1>Good</h1>
    </div>
  );
};
```

## Fragment example

**<React.Fragment></React.Fragment> === <></>**

Is a component created by React because JSX only can have one parent to avoid this we use Fragment also known as empty tag so you cannot pass any attributes or styles

## VIRTUAL_DOM

It is a repersentation/copy of actual DOM
React uses something called Reconciliation
so it uses a diff algotherem and compares two DOM tress and comes to consulsion which node in the tree to re-render instead of re-rendering entire DOM tree on every change

### Key in React

Suppose consider you have multiple `<div>` elements as childs of some node. if you add a new `<div>` to the list react do not know which things are changed while comparing because for react everything is same `<div>` elements so it re-renders entire list. if the type of elements are different like `<div>` and `<h1>` it is fine because they are 2 diff things. so react comes with key attribute, If you place a unique key to each `<div>`element even if the new `<div>` added react knows which things changed and only re-renders new item.

if you do-not have unique id then only use index as key but is not recommended.

## HOOKS

it is just a normal function written by some Facebook developers which have some certain job to do

## Lazy loading

While bundeling react as a single page application bundles all code into single file which increases bundle size .

### Corns of having single bundle file:

Take paytm as example there has many components lets say features
in end user perspective user journey in app, user might not go to every feature the odds are very less
lets say some user wants to book movies then he only checks movies component then he does not need files related to train / flight bookings and vice vera.
second is if we have multiple components and are image rich then as images are large and this adds weight to bundle

In this cases we can use some concept with different names as below

> > Lazy Loading, Chunking , Code Splitting , Dynamic Imports , Dynamic Bundling , On Demand Loading.

Here what happens is only when requested like clicking on particular component by end user then realted files are bundled . Which improves speed.
Below is the syntax

Syntax

```javascript
const Component = lazy(()=>import("../SomeComponent"))
 <Suspense
    fallback={()=><h1>loading</h1>}
 >
    <Component />
</Suspense>
```

Why we used suspense is, First thing react tries is while clicking on component it tries to render the component like mounting. but as we are dynamically importing downloading code from server takes some time, in that time react throws error because for reacts view there is no file it suspends the operation. so react provided something called suspense literally suspense )= . it takes a fallBack which shows in the meantime of downloading content from server.

### DATA

> `UI layer(every thing you see on ui) / Data Layer (Rest all happens) => Front end page (Single Page Application)`
> These two layers works indepandant and are in sync

## Prop Drilling

> Data is passing as chain from parent to child and so on...  
> for 2 or 3 levels it is ok but more levels it is waste and not good
> one change in prop of parent will re render all cards even they are not using that prop just passing to chid3 like that.

```
 Parent data
    Child 1 prop={data}
        Child 2 prop={data}
            Child 3 prop={data}
                Child 3 prop={data}
```

## Context

```javascript
Const ContextCreated = createContext({
   user:{}
})

ContextCreated.displayName = "Some Name"
```

Helps in debugging if we have multiple context this displayName helps us in dev tools.

We can override the default value of the context using Provider value prop

```javascript
const [user,setUser]=>useState({
    ....
})

<ContextCreated.Provider
    value={{
        user,
        setUser
    }} // Dynamically send will change default value
>
    <App>
</ContextCreated.Provider>
```

### In Functional component

```javascript
const data = useContext(ContextCreated);
```

> This data will have user and setUser all things passed to value prop

### In Class Component

```javascript
<ContextCreated.Consumer>{(contextData)=> return JSX}</ContextCreated.Consumer>
```

## Redux and Dev tools

Redux-Toolkit is updated version of redux
Redux-toolkit is core like having store

react-redux is bridge btw react app and store so we need 2 packages

> React Dev Tools component and profile tab play with it

> [All About Bundler Blog](https://dev.to/sayanide/the-what-why-and-how-of-javascript-bundlers-4po9#:~:text=A%20JavaScript%20bundler%20is%20a%20tool%20that%20helps,minimize%20HTTP%20requests%20and%20improve%20page%20load%20performance.)

## TESTING

### Types of testing

- Manual Testing
- Automation Testing
- End 2 End Testing -(Using Selinum , cypress) it just automates user flow in Application

- Unit Testing - Testing a small unit or function
- Integration Testing - Testing collection of units or flow of collection of components

### **Jest is javascript testing framework**

> There is something called testing library which includes react testing library and lot of testing frameworks

React testing library uses jest behind the sene so we need to install jest also because testing library is dependent on jest.

```
1. Install React Testing library
2. Install Jest
3. npx jest --init => to configure jest in our application it will create jest.config.js file
4. We need babel with jest because jest will not understand ES6 and imports and babel help in this case.
   Need to configure babel there are 2 options
   => .babelrc or babel.config.js, But .babelrc requires JSON but babel.config is Javascript
```

### **Simple JS testing**

```javascript
const product = (a,b)=> a\*b // JS File

test("Telling what this text case will be good here",()=>{
expect(product(2\*3)).toBe(6)
})
```

### React Testing

Jest will not understand JSX, So we will need another configuration in our pre babel configuration file so jest will understand.  
Here we are running our code in JS Dom , we have 2 options JS Dom and node environments.

Js Dom we are using in jest, it will not understand .png .jpeg, Because it will try to read it as Javascript only.  
Whenever Js Dom or Jest does not understand then we will make a mock file and return a string.

In jest config file there is something called module name mapper in that file we config like that if any .png .jpeg like files comes then use my mock file

```
moduleNameMapper:{
"\\.(jpg|png|svg|jpeg)$":"my mock file path"
}
```

Jest does not understand fetch or axios so we have to mock our api call
Jest is giving something called dummy function ,Here we are mocking API with fetch
as we know fetch will return a Promise with readable data and we will convert that data to json which will be another promise . This we are stimulating this in our fake api

#### Mock API Example

```javascript
const data = await fetch("someurl");
const res = await data.json();
global.fetch = jest.fn(() => {
  return Promise.resolve({
    json: () => Promise.resolve(OUR_DUMMY_DATA),
  });
});
```

> There is something **waitFor()** a function so it waits till something load

- We can fire all events by mocking events for the purpose of testing
- we use testIds to access elements in our JSX

## Some Questions

**Why need state ?**

> React only tracks all variables which are in state and re-renders components. If you did not use state then the data will be updated whatever you write but not reflect in UI

**What is useState ?**

> useState is a React Hook or you can say js function which handles state and returns an array with [variable,function to update variable]

**Why React Is Fast?**

> It is because of fast DOM manupulation using Virtual DOM and reconsilation algithermerm

**What is a Microservice? , What is Monolith architecture and What is the difference between Monolith and Microservice?**

> Monolith is like doing the whole school project by yourself.  
> You need to make a big poster for your project, and you do everything: drawing pictures, writing, cutting, and gluing all by yourself. If you make a mistake in one part, like the writing, you have to start over or fix the whole thing, because it’s all one big project.

> Microservice is like working with friends on different parts of the project.  
> You and your friends each do one part of the project: one person draws the pictures, one person writes the words, one person cuts the paper, and another person glues everything. If one part, like the pictures, needs fixing, you can just fix that part without touching the other parts. Everyone is working on a small piece, and it all comes together in the end!

#### The Difference:

> Monolith is like doing everything by yourself, all in one big project. If one part is wrong, it’s hard to fix.  
> Microservices is like working with friends, where each person does one part. If something goes wrong, only that part gets fixed, and the rest is fine!

> [Medium Article](https://medium.com/design-microservices-architecture-with-patterns/architecture-comparison-monolithic-vs-microservices-4109265c4806)

**Why do we need a useEffect Hook?**

> The useEffect Hook is needed to handle side effects in functional components, such as fetching data, updating the DOM, and setting up subscriptions or timers. It allows you to perform actions that are necessary during the lifecycle of a component, rather than on specific user interactions or DOM events

**What is Optional Chaining?**

> optional chaining (?.) . It tries to access object property or calls a function, If it is undefined or null then it return undefined instead of throwing error

```javascript
const adventurer = {
  name: "Alice",
  cat: {
    name: "Dinah",
  },
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// Expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// Expected output: undefined
```

> [MDN Document For Optional Chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

**What is the difference between JS expression and JS statement**

> [Read This Dev Community Blog](https://dev.to/jeetvora331/javascript-expression-vs-statement-and-their-importance-in-react-2k8e)

**What is CORS?**

> [MDN Document](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

**What is async and await?**

> Async and await are features in programming languages that allow for asynchronous, non-blocking code to be written in a way that resembles traditional synchronous code. This makes it easier for developers to write and understand asynchronous operations without complicating the code with callbacks or other complex structures.  
> [Youtube](https://www.youtube.com/watch?v=6nv3qy3oNkc)

**Why CDN is used to store images? (Swiggy Uses it)**

> [Read This Blog](https://dev.to/uploadcare/the-complete-guide-to-image-cdns-3ao)

**What would happen if we do console.log(useState())?**

**How will useEffect behave if we don't add a dependency array ?**

> If will trigger only one time when the component first time called or you can say it adds as component dit mount.

**What is SPA(Single Page Application)?**

> A single page application (SPA) is a type of web application that loads a single web document and updates the content dynamically via JavaScript APIs such as Fetch, without requiring full page reloads.
> [MDN Docs](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

**What is difference between Client Side Routing and Server Side Routing?**

> Client-side routing occurs in the browser, updating the URL and content without requesting a new page from the server. This approach is commonly used in single-page applications (SPAs) to enable smooth transitions between different views or states within the same page, enhancing user experience by avoiding full page reloads.  
> Server-side routing, on the other hand, involves the server handling URL requests and sending back the relevant HTML content for each page. When a user navigates to a new page, the browser sends a request to the server, which responds with the appropriate HTML, leading to a full page refresh.  
> [Blog for reference](https://medium.com/@wilbo/server-side-vs-client-side-routing-71d710e9227f)


**We can make componentDidMount as async but we cannot make callBack function in useEffect async why?**

> **useEffect** is designed like it thinks its callBack function will return a function to clean up or undefined. if we place async for useEffect callBack which returns a promise.This causes unexpected behaviour because the expected use case of useEffect is different.  
> On the other hand **componentDidMount**() is a function Called after the component has rendered and the DOM is ready. You can use async functions within componentDidMount because it doesn’t have the same requirement for a cleanup function.  
> **ComponentDidMount**: Inherently synchronous. You can call an async function within it to perform side effects without needing to return a cleanup function  
> [For more refer This blog explains neatly](https://dev.to/niketanwadaskar/why-cant-we-use-async-with-useeffect-but-can-with-componentdidmount-45be)

**Why when we place "," at last record in JSON throws error?**

> JSON has a strict rule: there should never be a comma after the last item in an object or array. It expects that after the last item, there is nothing left to separate.

**Why type module in js ?**

```
When you use the "type": "module" option in your package.json file (or in your HTML <script> tag), you tell the browser or Node.js that your JavaScript files are modules and you want to use features like:

import: You can bring in code from another file to use in your current file.

export: You can send some code (functions, variables, etc.) from one file so other files can use it.

Without telling the system, "Hey, this is a module," it wouldn't know you’re trying to use the modern import/export features. So, "type": "module" is a way to tell JavaScript that you are using modules and want to organize your code into different files.
```

**Why reportWebVitals comes when we create react app**

> [Medium Article](https://medium.com/@larrydev/understanding-web-vitals-in-react-a-guide-to-improving-user-experience-and-performance-89f7c898b504)

**What is LRU cache ?**

```
Imagine you have a toy box with a limited amount of space, and you have a lot of toys that you like to play with. You can only keep a few toys in the box at a time because it’s small.

What happens if the box gets full?
When you get a new toy, you need to make space in the box. So, you give away or remove the oldest toy you haven’t played with in a while to make room for the new one. This way, the toys in your box are always the ones you play with the most!

That’s exactly how an LRU (Least Recently Used) Cache works!
Cache is like a special toy box for storing things (like toys or information) that you use a lot.

LRU means "Least Recently Used," which is a fancy way of saying, the oldest toy (or information) gets removed first when the box is full.

So, when you run out of space in your toy box (or cache), the least recently used toy (or the one you haven’t played with in a while) is the first to go, making room for the new one you want to keep.

In short:
An LRU Cache is a smart toy box that remembers which toys you played with last and makes sure it only keeps the toys you play with the most. If you don’t play with a toy for a long time, it will be the first one to go!

```

### SOME_IMPORTANT_KEYWORD

> React-key Reconciliation [Reference Link](https://legacy.reactjs.org/docs/reconciliation.html)  
> React-Fiber  
> Can we run react without JSX? Yes [View This Link to get more clarification](https://legacy.reactjs.org/docs/react-without-jsx.html)
> React-Dom package is responsible for DOM manupulation
> [All About Bundler Blog](https://dev.to/sayanide/the-what-why-and-how-of-javascript-bundlers-4po9#:~:text=A%20JavaScript%20bundler%20is%20a%20tool%20that%20helps,minimize%20HTTP%20requests%20and%20improve%20page%20load%20performance.)
