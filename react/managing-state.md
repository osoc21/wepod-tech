# Managing the state in React

Some thought & recommandation

## Let React do it

Basically, "useState" anything that need to change in your components. It's not complex to do and it will save a lot of headache.

## Think about the good level

Most probably you'll endup with a 'main' state (let's say - a list of files and a path) that are useful to every components (ex: a list view, a grid/image view, a detail view, a map view).

In that situation, all child components just receive props (and the setter if needed), and all the code to "manage" that list stays in the main component.

## Split data problems from rendering problems

You want your state to be an array of objects - not React components. This means every component can then decide how to show the info.

## Where to load data

You generally don't want to reload data at each render. React default model for this is to put your data loading in a single async function, call it inside "useEffect" and use a state setter as result:


```JavaScript
 async function loadData() {
   // do stuff
   setData(data)
 }

const [data, setData] = useState([])

useEffect(() => {
       loadData();
    }, [<relevant props>])
```

useEffect garantee that the method will only be called once each time the value provided in the array change. For example, if we load data for the currentPath (a props), we can put props.currentPath in the array, ensuring that we'll call again should that props change.

## Enrich on the data

If you end up "enriching" the data, put it back on the main array:

- A "isFolder" boolean
- A "human readable" title
- A imageUrl for images
- Tags
- ...

Try to avoid having a top of functions to do this everywhere - get them at high level & put all what you need on the array, so that rendering is easy. Remember that in JavaScript an Object is an associative array - you can plug any properties (or functions) you want on it.

## Async will bite you

A lot of operations in Solid & React are asynchronous - that means the method returns immediatly but the code will be executed "later". Two patterns for that:

### then

Any async method returns a Promise object with a then() method that will be called when done:

```JavaScript
  asyncFunction(param).then((result) => {
    // can use result here
  })
  // but this will be executed first
```

Good candidates to use in then() are state setters.

### Await

You can also put the "await" keyword before any method to "force" it to be sync. Fun fact, this can only used inside an async method itself:


```JavaScript
async function loadData() {
   // call an async method:

   let result = await asyncFunction(param)
   // result can be used here
   setData(data)
 }
```

Note that this would not work in a forEach:

```JavaScript
async function loadData() {
   let someArray = [...]

   someArray.forEach((item) => {
     let result = await asyncFunction(param)
     // does not compile! 
   })
 }
```

Why? the anymous function called by forEach is not async - so you can't call "await" inside. For to the rescue:

```JavaScript
async function loadData() {
   let someArray = [...]

   for(item of someArray {
     let result = await asyncFunction(param)
     // We're good, can use result here 
   })
 }
```

### useState

By the say, the setter you get through useState are async too so code like this will be surprising:

```JavaScript
const [valid, setValid] = useState(false)

setValid(true)
console.log(valid) // => false as setValid is async
```

Race conditions are messy - try not to rely on them, if needed use some "await" to go back to sync code.
