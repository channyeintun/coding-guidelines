## General Rules Must Follow In Project

### 1. Variables
#### Naming convention should be consistent
Choose only a single naming convention to be used in the project. Use constant case for constants. Use upper camel case for component names (React). Use camel case for variable and function's names. Use dash case for route name.
| Formatting | Name             |
|------------|------------------|
| TWO_WORDS  | Constant case    |
| twoWords   | Camel case       |
| TwoWords   | Upper Camel case |
| two-words  | Dash case        |
#### Use const and let instead of var
Using var provides variable hoisting which is error-prone. Using const and let provides block scope that allows us to write clean and less error-prone code.

#### Give your variables meaningful and pronounceable names
Have you ever felt guilty of taking a shortcut to name your variables x , y , z  & i or naming your functions func1(), temp(), etc. Did you have a hard time understanding that same piece of code when you came back to it a few weeks later? Did you have to supplement that piece of code with inline code commentary? Please don’t do it — there is a better approach — self-documenting code.

#### Example
:x:
```javascript
let t
```
:white_check_mark:
```javascript
let elapsedTimeInDays
```
#### Use the same vocabulary for the same type of variable
:x:  
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```
:white_check_mark:  
```javascript
getUser();
```
#### Use explanatory variables
:x:
```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```
:white_check_mark:
```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
#### Avoid Mental Mapping
:x:
```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```
:white_check_mark:
```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```
#### Don't add unneeded context
:x:
```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```
:white_check_mark:
```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```
#### Use default parameters instead of short circuiting or conditionals
Default parameters are often cleaner than short circuiting. Be aware that if you use them, your function will only provide default values for undefined arguments. Other "falsy" values such as '', "", false, null, 0, and NaN, will not be replaced by a default value.  
  
:x:
```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```
:white_check_mark:
```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```
#### Use Searchable Names
Avoid using magic numbers in your code. Opt for searchable, named constants. Do not use single-letter names for constants since they can appear in many places and therefore are not easily searchable.
#### Example
:x: 
```javascript
if ( speed > 50 ) {
  // body of if statement
}
```
:white_check_mark:
```javascript
let SPEED_LIMIT = 50
if ( speed > SPEED_LIMIT ) {
  // body of if statement
}
```
### 2.  Functions
Function names should be verbs if the function changes the state of the program, and nouns if they're used to return a certain value.
#### Keep them Small
Functions should be small, really small. They should rarely be 20 lines long. The longer a function gets, it is more likely it is to do multiple things and have side effects.

#### Expressive Naming

:x: 
```javascript
const setup=(text,subText) => <AlertDialog text={text} subText={subText} />
```
:white_check_mark:
```javascript
const createAlertDialog=(text,subText) => <AlertDialog text={text} subText={subText} />
```

#### Function arguments (2 or fewer ideally)
Limiting the amount of function parameters is incredibly important because it makes testing your function easier.  
  
One or two arguments is the ideal case, and three should be avoided if possible.  
  
 You can use an object if you are finding yourself needing a lot of arguments.

  1. When someone looks at the function signature, it's immediately clear what properties are being used.  
  2. It can be used to simulate named parameters.  
  3. Linters can warn you about unused properties, which would be impossible without destructuring.  

  :x:
  ```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
  ```
  :white_check_mark:
  ```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
  ```
#### Functions should do one thing (SRP)
This is by far the most important rule in software engineering. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, it can be refactored easily and your code will read much cleaner. If you take nothing else away from this guide other than this, you'll be ahead of many developers.  
  
:x:
```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```
:white_check_mark:
```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
#### Function names should say what they do
:x:
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```
:white_check_mark:
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
#### Functions should only be one level of abstraction
When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.  
  
:x:
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```
:white_check_mark:
```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

#### Remove duplicate code
Do your absolute best to avoid duplicate code. Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.  
  
Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.  
  
Getting the abstraction right is critical. Bad abstractions can be worse than duplicate code, so be careful!  
  
Don't repeat yourself! Do not create your own error prone utility functions. Use libraries like Lodash and Ramda.

#### Set default objects with Object.assign
:x:
```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```
:white_check_mark:
```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

#### Don't use flags as function parameters
Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.  
  
:x:
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```
:white_check_mark:
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

### Avoid Side Effects
A side effect could be writing to a file, modifying some global variable.  
  
:x:
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```
:white_check_mark:
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
In JavaScript, some values are unchangeable (immutable) and some are changeable (mutable). Objects and arrays are two kinds of mutable values so it's important to handle them carefully when they're passed as parameters to a function.  
  
Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are great libraries e.g Immutable.js that allow this kind of programming approach to be fast and not as memory intensive.  
  
:x:
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```
:white_check_mark:
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```
### Never do Monkey Patching
Monkey patching makes the team to get into the big trouble. Very hard to debug the bugs that it makes.  
  
:x:
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```
:white_check_mark:
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

### Favor functional programming over imperative programming
JavaScript isn't a functional language in the way that Haskell is, but it has a functional flavor to it. Functional languages can be cleaner and easier to test. Favor this style of programming when you can.  
  
:x:
```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```
:white_check_space:
```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
``` 
### Encapsulate conditionals
:x:
```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```
:white_check_space:
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
### Avoid negative conditionals
We can use the decorator in functionnal programming style.  

:x:
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (!isDOMNodePresent(node)) {
  // ...
}
```
:white_check_mark:
```javascript
function isDOMNodePresent(node) {
  // ...
}

const not=(func)=>params=>!func(params)

const isDOMNodeNotPresent=not(isDOMNodePresent)

if (isDOMNodeNotPresent(node)) {
  // ...
}
```
 ### Avoid Conditionals
 Sometimes, you can avoid conditionals like if/else or switch by using objects and dynamic keys.  
   
:x:
 ```javascript
 function getColorHashCode(name){
    switch (name){
        case "Red":
            return "#ff0000";
        case "Green":
            return "#00ff00";
        case "Blue":
            return "#0000ff";
    }
 }

 getColorHashCode("Red")
 ```
 :white_check_mark:
 ```javascript
const colors={
    Red:"#ff0000",
    Green:"#00ff00",
    Blue:"#0000ff"
}

const getColorHashCode=(name)=>colors[name]

getColorHashCode("Red")
 ```

 ### Avoid type-checking
If you need type checking, just move to TypeScript or use Flow. TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale. FLOW IS A STATIC TYPE CHECKER FOR JAVASCRIPT. Flow is a static type checker for javascript but not a language.  
  
:x:
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```
:white_check_mark:
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```
### Don't over-optimize
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time.  
  
:x:
```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```
:white_check_mark:
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

### Remove dead code
Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase. If it's not being called, get rid of it! It will still be safe in your version history if you still need it. It can cost time and money to read it and to write test cases for it.  
  
:x:
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```
:white_check_mark:
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

### Async/Await are even cleaner than Promise chaining
:x:
```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```
:white_mark_check:
```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

### Error Handling
#### Don't ignore caught errors
:x:
```javascript
try {
  functionThatMightThrow();
} catch (error) {
 // this empty can lead you to the hell
}
```
:white_check_mark:
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.error(error);
}
```

### Comments
#### Only comment things that have business logic complexity.
Good code mostly documents itself.  
  
:x:
```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```
:white_check_mark:
```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

#### Don't leave commented out code in your codebase
Version control exists for a reason. Leave old code in your history.  
  
:x:
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```
:white_check_mark:
```javascript
doStuff();
```
