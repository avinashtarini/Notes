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
        <h1 id="someIdOne" >First Line</h1>
         <h1 id="someIdTwo" >Second Line</h1>
    </div>
)
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
const FunctionalExample =()=> (
    <div>
        <h1> Returns some JSX</h1>
    </div>
)
```

## Class Components
Class Components is normal js class 
render() method is mandatory method in class component 
render returns some JSX

### Basic example
class BasicClass extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
    }
 
    render(){
        return(
            <div>
                <h1></h1>
            </div>
        )
    }
}
### Code example of React element
This is Element 

```javascript 
const jsxCodeOfAbove = (
    <div>
        <h1 id="someIdOne" >First Line</h1>
         <h1 id="someIdTwo" >Second Line</h1>
    </div>
)
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
React will do these in 2 phases 
> ### Render phase and Commit phase
Refer the link which illurates the component life cycle diagrams  
[Component life cycle diagram reference link react](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  
Lets first understand life cycle methods using class components
### Examples to understand what are called first
> **Constructor => render => componentDidMount => render(if anything changed) => componentDidUpdate => componentWillUnmount**

The order component cycle will call  
> parent-constructor => parent-render => here react returns JSX which contains another component so react completes that component cycle so => child-constructor=>
    child-render => child-did-mount => parent-did-mount

```javascript 
class Parent extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
        console.log('parent-constructor')
    }
    componentDidMount(){
        console.log('parent-did-mount')
    }
    render(){
        console.log('parent-render')
        return(
            <div>
                <h1></h1>
                <Child /> 
            </div>
        )
    }
}

class Child extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
        console.log('child-constructor')
    }
    componentDidMount(){
        console.log('child-did-mount')
    }
    render(){
        console.log('child-render')
        return(
            <div>
                <h1></h1>
            </div>
        )
    }
}
```
Lets look at if there are 2 child components
Order of react components call 
>parent-constructor =>parent-render => child-constructor-1 => child-render-1 => child-constructor-2 => child-render-2=>child-diMount-2=>child-diMount-1=>parent-diMount-1
 
```javascript
class Parent extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
        console.log('parent-constructor')
    }
    componentDidMount(){
        console.log('parent-did-mount')
    }
    render(){
        console.log('parent-render')
        return(
            <div>
                <h1></h1>
                <Child num={1} /> 
                <Child num={2} /> 
            </div>
        )
    }
}

class Child extends React.Component{
    constructor(props){
        super(props)
        this.state = {}
        console.log('child-constructor'+this.props.num)
    }
    componentDidMount(){
        console.log('child-did-mount'+this.props.num)
    }
    render(){
        console.log('child-render'+this.props.num)
        return(
            <div>
                <h1></h1>
            </div>
        )
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
    componentDidMount(){
        console.log('after first render it calls')
    }
    componentDidUpdate(){
        console.log('after after every re-render')
    }
    componentWillUpdate(){
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
>_**useEffect is not equvalent to componentDidMount never compare functional components with component life cycles 
we can do every phase using useEffect see example**_

```javascript

const MyComponent = ()=>{

    const [something] = useState('') // initilizing state

    useEffect(()=>{
        // This will call after every re-render like componentDidUpdate
    })
    useEffect(()=>{
        // after first render it calls like componentDidMount
        return()=>{
            // called before component is leaving like componentWillUnmount
        }
    },[])
    
    useEffect(()=>{
        // This is equal to compare state with prevState kind of thing in componentDidUpdate
    },[something])

    return(
        <div>
            <h1>Good</h1>
        </div>
    )
}


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

## Some Questions

**Why need state ?**   
>React only tracks all variables which are in state and re-renders components. If you did not use state then the data will be updated whatever you write but not reflect in UI 

**What is useState ?** 
>useState is a React Hook or you can say js function which handles state and returns an array with [variable,function to update variable]

**Why React Is Fast?**  
>It is because of fast DOM manupulation

What is a Microservice?  
What is Monolith architecture?  
What is the difference between Monolith and Microservice?  
Why do we need a useEffect Hook?  
    useEffect is called after component is rendered  
What is Optional Chaining?  
What is Shimmer UI?  
What is the difference between JS expression and JS statement  
What is Conditional Rendering, explain with a code example  
What is CORS?  
What is async and await?  
What is the use of `const json = await data.json();`  
Why CDN is used to store images? (Swiggy Uses it)  
What are various ways to add images into our App? Explain with code examples  
What would happen if we do console.log(useState())?  
How will useEffect behave if we don't add a dependency array ?  
What is SPA(Single Page Application)?  
What is difference between Client Side Routing and Server Side Routing?  
We can make componentDidMount as async but we cannot make callBack function in useEffect async why?    

### SOME_IMPORTANT_KEYWORD
> React-key Reconciliation [Reference Link](https://legacy.reactjs.org/docs/reconciliation.html)    
React-Fiber  
Can we run react without JSX? Yes [View This Link to get more clarification](https://legacy.reactjs.org/docs/react-without-jsx.html)
React-Dom package is responsible for DOM manupalation  
