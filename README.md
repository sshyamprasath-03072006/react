# 🎯 JAVASCRIPT + REACT LAB GUIDE
## Concept-to-Question Mapping with Complete Solutions

---

## 📋 QUICK REFERENCE: Which Concepts Apply to Which Questions

| Lab Question | Core Concepts | Difficulty |
|--------------|---------------|------------|
| **Q1** - Calculate Fare | `this` context, `.apply()`, arrow functions | ⭐⭐ |
| **Q2** - Header/Hero Components | Functional vs Class components, JSX, props | ⭐ |
| **Q3** - UserList | Arrays, `.map()`, conditional rendering, props | ⭐⭐ |
| **Q4** - Feedback Form | Forms, `useState`, validation, controlled components | ⭐⭐⭐ |
| **Q5** - Shopping Cart | Class components, state management, conditional rendering | ⭐⭐⭐ |
| **Q6** - Timer | Lifecycle methods, `setInterval`, cleanup | ⭐⭐ |
| **Q7** - Hero Manager | Context API, `useReducer`, complex state | ⭐⭐⭐⭐ |
| **Q9** - Advice Generator | `async/await`, API calls, error handling, `useEffect` | ⭐⭐⭐ |

---

## 🔥 QUESTION 1: CALCULATE FARE (`.apply()` and `this`)

### **Core JavaScript Concepts**
- **`this` keyword** - refers to the object calling the function
- **`.apply()`** - calls a function with a given `this` value and arguments as an array
- **Arrow functions** - don't have their own `this`

### **The Problem**
You have a `calculateFare` function that needs access to a `tax` property. Different drivers have different tax rates.

### **Key Concepts Explained**

#### 1️⃣ Understanding `this`
```js
const person = {
  name: "Ali",
  greet: function() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.greet(); // "Hello, I'm Ali"
// 'this' refers to 'person' object
```

#### 2️⃣ `.apply()` Method
```js
// Syntax: function.apply(thisArg, [arg1, arg2, ...])

function introduce(greeting, punctuation) {
  return `${greeting}, I'm ${this.name}${punctuation}`;
}

const student = { name: "Sara" };
const result = introduce.apply(student, ["Hi", "!"]);
// result = "Hi, I'm Sara!"
```

### **Complete Solution with Explanation**
```js
// PART 1: Define the fare calculation function
function calculateFare(distance, rate, extraCharges) {
  // Calculate base fare
  const baseFare = distance * rate + extraCharges;
  
  // Add tax from 'this' object
  // this.tax comes from the driver object
  const totalFare = baseFare + (baseFare * this.tax / 100);
  
  return totalFare;
} // ⚠️ CRITICAL: This brace was missing in your code!

// PART 2: Create driver objects with different tax rates
const driver1 = { name: 'Kiran', tax: 5 };
const driver2 = { name: 'Sneha', tax: 12 };

// PART 3: Prepare arguments
const args = [10, 12, 30]; // [distance, rate, extraCharges]

// PART 4: Call function with different 'this' contexts
const fare1 = calculateFare.apply(driver1, args);
// Inside calculateFare: this.tax = 5 (from driver1)
// baseFare = 10 * 12 + 30 = 150
// tax = 150 * 5/100 = 7.5
// total = 150 + 7.5 = 157.5

const fare2 = calculateFare.apply(driver2, args);
// Inside calculateFare: this.tax = 12 (from driver2)
// baseFare = 150 (same calculation)
// tax = 150 * 12/100 = 18
// total = 150 + 18 = 168

console.log(`Driver 1 fare: $${fare1}`); // $157.5
console.log(`Driver 2 fare: $${fare2}`); // $168
```

### **Why Your Code Failed**
```js
// ❌ WRONG - Missing closing brace
function calculateFare(distance, rate, extraCharges) {
  const baseFare = distance * rate + extraCharges;
  const totalFare = baseFare + (baseFare * this.tax / 100);
  return totalFare;
// Missing } here!

// ✅ CORRECT
function calculateFare(distance, rate, extraCharges) {
  const baseFare = distance * rate + extraCharges;
  const totalFare = baseFare + (baseFare * this.tax / 100);
  return totalFare;
} // ← Closing brace present
```

### **Alternative: Using `.call()` and `.bind()`**
```js
// Method 1: .call() - pass arguments individually
const fare1 = calculateFare.call(driver1, 10, 12, 30);

// Method 2: .bind() - create new function with fixed 'this'
const calculateFareForDriver1 = calculateFare.bind(driver1);
const fare1 = calculateFareForDriver1(10, 12, 30);
```

---

## 🔥 QUESTION 2: HEADER & HERO COMPONENTS

### **Core React Concepts**
- Functional components
- Class components
- JSX syntax
- Component composition
- Import/Export

### **File Structure**
```
src/
├── App.js
└── components/
    ├── Header.js
    └── Hero.js
```

### **Solution 1: Header.js (Functional Component)**
```jsx
// Header.js - FUNCTIONAL COMPONENT
import React from 'react';

function Header() {
  return (
    <div id="Header">
      <h1>Hello John</h1>
    </div>
  );
}

// Named export
export default Header;
```

**Key Points:**
- Functional component = JavaScript function that returns JSX
- Must return ONE root element (`<div>` wraps everything)
- Use `export default` to make it available to other files

### **Solution 2: Hero.js (Class Component)**
```jsx
// Hero.js - CLASS COMPONENT
import React, { Component } from 'react';

class Hero extends Component {
  render() {
    return (
      <div id="Hero">
        <h2>Welcome to my website!</h2>
        <p>Learn and grow</p>
      </div>
    );
  }
}

export default Hero;
```

**Key Points:**
- Class component extends `Component`
- Must have `render()` method
- `render()` returns JSX
- Modern React prefers functional components

### **Solution 3: App.js (Composition)**
```jsx
// App.js - PARENT COMPONENT
import React from 'react';
import Header from './components/Header';
import Hero from './components/Hero';

function App() {
  return (
    <div>
      <Header />
      <Hero />
    </div>
  );
}

export default App;
```

**Key Points:**
- Import components using relative paths (`./components/Header`)
- Use components like HTML tags (`<Header />`)
- Self-closing tags when no children

### **Functional vs Class Components**
```jsx
// FUNCTIONAL (Modern, Preferred)
function Greeting() {
  return <h1>Hello</h1>;
}

// CLASS (Legacy, Still Supported)
class Greeting extends Component {
  render() {
    return <h1>Hello</h1>;
  }
}

// ARROW FUNCTION (Also Functional)
const Greeting = () => <h1>Hello</h1>;
```

---

## 🔥 QUESTION 3: USER LIST (Arrays & Conditional Rendering)

### **Core Concepts**
- Props (passing data to components)
- `.map()` for rendering lists
- Conditional rendering (`? :` and `&&`)
- Default props

### **Key JavaScript: `.map()` Review**
```js
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
// doubled = [2, 4, 6]

// In React, .map() creates JSX elements
const listItems = numbers.map(num => <li>{num}</li>);
```

### **Complete Solution: UserList.js**
```jsx
import React from 'react';

const UserList = ({ users = [] }) => {
  // PATTERN 1: Early return for empty state
  if (!users.length) {
    return <div>No users available.</div>;
  }

  // PATTERN 2: Render list with .map()
  return (
    <ul id="user-list">
      {users.map((user, index) => (
        <li key={index} data-testid="user-item">
          {/* PATTERN 3: Conditional rendering with ternary */}
          {user.name} {user.isActive ? `(${user.role})` : '(Inactive)'}
        </li>
      ))}
    </ul>
  );
};

export default UserList;
```

### **Breaking Down the Logic**

#### Step 1: Default Props
```jsx
const UserList = ({ users = [] }) => {
  // If 'users' is undefined, use empty array []
  // Prevents errors when accessing users.length
```

#### Step 2: Guard Clause
```jsx
if (!users.length) {
  return <div>No users available.</div>;
}
// If array is empty, show this message and exit early
```

#### Step 3: Map Over Users
```jsx
{users.map((user, index) => (
  // For each user object, create an <li>
  <li key={index}>
    {/* Content here */}
  </li>
))}
```

#### Step 4: Conditional Display
```jsx
{user.name} {user.isActive ? `(${user.role})` : '(Inactive)'}

// If user.isActive is true:  "Alice (Admin)"
// If user.isActive is false: "Bob (Inactive)"
```

### **App.js Usage**
```jsx
import React from 'react';
import UserList from './components/UserList';

const App = () => {
  // Data array
  const users = [
    { name: 'Alice', role: 'Admin', isActive: true },
    { name: 'Bob', role: 'User', isActive: false },
    { name: 'Charlie', role: 'User', isActive: true },
  ];

  return (
    <div>
      <h1>User List</h1>
      <UserList users={users} />
    </div>
  );
};

export default App;
```

### **Common Patterns for Conditional Rendering**
```jsx
// 1. Ternary operator (if/else)
{isLoggedIn ? <Dashboard /> : <Login />}

// 2. Logical AND (only if true)
{showMessage && <p>Welcome!</p>}

// 3. If/else before return
if (loading) return <p>Loading...</p>;
if (error) return <p>Error: {error}</p>;
return <DataDisplay />;

// 4. Inline with variables
const message = isActive ? "(Active)" : "(Inactive)";
return <p>{message}</p>;
```

---

## 🔥 QUESTION 4: FEEDBACK FORM (Forms + Validation)

### **Core Concepts**
- Controlled components (form inputs tied to state)
- `useState` hook
- Event handling (`onChange`, `onSubmit`)
- Form validation logic
- Conditional error messages

### **Complete Solution: FeedbackForm.jsx**
```jsx
import React, { useState } from 'react';

// Mock database of registered users
const currentUsers = [
  { email: 'user1@example.com', userId: 'user1' },
  { email: 'user2@example.com', userId: 'user2' },
  { email: 'user3@example.com', userId: 'user3' }
];

function FeedbackForm() {
  // STATE 1: Form data (all fields in one object)
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: '',
    userId: '',
    message: ''
  });

  // STATE 2: Messages
  const [errorMessage, setErrorMessage] = useState('');
  const [successMessage, setSuccessMessage] = useState('');

  // HANDLER 1: Update form field
  const handleChange = (event) => {
    setFormData({
      ...formData, // Keep all other fields
      [event.target.name]: event.target.value // Update this field
    });
    
    // Clear messages when user types
    setErrorMessage('');
    setSuccessMessage('');
  };

  // HANDLER 2: Form submission
  const handleSubmit = (event) => {
    event.preventDefault(); // Prevent page reload!

    // Reset messages
    setErrorMessage('');
    setSuccessMessage('');

    // VALIDATION 1: Check if email exists
    const matchedUser = currentUsers.find(
      user => user.email === formData.email
    );

    if (!matchedUser) {
      setErrorMessage('You are not registered, sign up to continue');
      return; // Stop execution
    }

    // VALIDATION 2: Check if userId matches email
    if (matchedUser.userId !== formData.userId) {
      setErrorMessage('User ID does not match the provided email');
      return;
    }

    // SUCCESS: All validations passed
    setSuccessMessage('Feedback submitted successfully!');
  };

  return (
    <div>
      <h1>Feedback Form</h1>
      <form onSubmit={handleSubmit} data-testid="feedback-form">
        {/* NAME INPUT */}
        <div>
          <label htmlFor="name">Name:</label><br/>
          <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChange}
            data-testid="input-name"
          />
        </div>

        {/* EMAIL INPUT */}
        <div>
          <label htmlFor="email">Email:</label><br/>
          <input
            type="email"
            id="email"
            name="name"
            value={formData.email}
            onChange={handleChange}
            data-testid="input-email"
          />
        </div>

        {/* PHONE INPUT */}
        <div>
          <label htmlFor="phone">Phone:</label><br/>
          <input
            type="tel"
            id="phone"
            name="phone"
            value={formData.phone}
            onChange={handleChange}
            data-testid="input-phone"
          />
        </div>

        {/* USER ID INPUT */}
        <div>
          <label htmlFor="userId">User ID:</label><br/>
          <input
            type="text"
            id="userId"
            name="userId"
            value={formData.userId}
            onChange={handleChange}
            data-testid="input-userId"
          />
        </div>

        {/* MESSAGE TEXTAREA */}
        <div>
          <label htmlFor="message">Message:</label><br/>
          <textarea
            id="message"
            name="message"
            value={formData.message}
            onChange={handleChange}
            data-testid="input-message"
          />
        </div>

        <br/>
        <button type="submit" data-testid="submit-button">Submit</button>
      </form>

      {/* CONDITIONAL MESSAGES */}
      {errorMessage && (
        <p data-testid="error-message" style={{ color: 'red' }}>
          {errorMessage}
        </p>
      )}
      {successMessage && (
        <p data-testid="success-message" style={{ color: 'green' }}>
          {successMessage}
        </p>
      )}
    </div>
  );
}

export default FeedbackForm;
```

### **Understanding Controlled Components**
```jsx
// CONTROLLED: React state is "source of truth"
const [name, setName] = useState('');

<input
  value={name}              // Value from state
  onChange={(e) => setName(e.target.value)} // Update state
/>

// UNCONTROLLED: DOM is "source of truth" (avoid this)
<input ref={inputRef} /> // Access via ref later
```

### **Form Data Pattern: Single State Object**
```jsx
// PATTERN 1: Individual states (gets messy with many fields)
const [name, setName] = useState('');
const [email, setEmail] = useState('');
const [phone, setPhone] = useState('');

// PATTERN 2: Single object (cleaner!)
const [formData, setFormData] = useState({
  name: '',
  email: '',
  phone: ''
});

// Update specific field
const handleChange = (e) => {
  setFormData({
    ...formData, // Spread existing values
    [e.target.name]: e.target.value // Update this field
  });
};
```

### **Validation Strategies**
```jsx
// STRATEGY 1: Validate on submit (this question uses this)
const handleSubmit = (e) => {
  e.preventDefault();
  if (!validateForm()) return; // Stop if invalid
  // Submit data
};

// STRATEGY 2: Real-time validation (advanced)
const [errors, setErrors] = useState({});

const handleChange = (e) => {
  const { name, value } = e.target;
  setFormData({ ...formData, [name]: value });
  
  // Validate this field immediately
  if (name === 'email' && !value.includes('@')) {
    setErrors({ ...errors, email: 'Invalid email' });
  } else {
    const { [name]: removed, ...rest } = errors;
    setErrors(rest);
  }
};
```

---

## 🔥 QUESTION 5: SHOPPING CART (Class Components + State)

### **Core Concepts**
- Class component state management
- `setState()` method
- Array manipulation (`.some()`, spread operator)
- Parent-child communication via props

### **Complete Solution: ShoppingCart.jsx**
```jsx
import React, { Component } from 'react';
import ProductList from './ProductList';

class ShoppingCart extends Component {
  // STEP 1: Initialize state
  state = {
    selectedProducts: [], // Array of added products
    cartFull: false,      // Flag for cart limit
  };

  // STEP 2: Method to add product
  addToCart = (product) => {
    // CHECK 1: Cart limit (max 2 products)
    if (this.state.selectedProducts.length >= 2) {
      this.setState({ cartFull: true });
      return; // Exit early
    }

    // CHECK 2: Product already in cart?
    const isAlreadySelected = this.state.selectedProducts.some(
      (item) => item.name === product.name
    );
    
    if (!isAlreadySelected) {
      // Add product to cart
      this.setState((prevState) => ({
        selectedProducts: [...prevState.selectedProducts, product],
        cartFull: false,
      }));
    }
  };

  // STEP 3: Render UI
  render() {
    const { selectedProducts, cartFull } = this.state;
    
    return (
      <div>
        <h2>Products</h2>
        
        {/* Show message if cart is full */}
        {cartFull && <h2 className="message">The cart is full</h2>}
        
        {/* Pass addToCart function as prop */}
        <ProductList addToCart={this.addToCart} />
        
        <h3>Cart</h3>
        <ul>
          {selectedProducts.map((product, index) => (
            <li key={index}>
              {product.name} - ${product.price}
            </li>
          ))}
        </ul>
        
        {/* Show message if trying to add duplicate */}
        {selectedProducts.length > 0 && !cartFull && (
          <h2 className="message">Product is already in the cart</h2>
        )}
      </div>
    );
  }
}

export default ShoppingCart;
```

### **ProductList.jsx (Child Component)**
```jsx
import React, { Component } from 'react';

class ProductList extends Component {
  state = {
    productList: [
      { name: 'Product 1', price: 10 },
      { name: 'Product 2', price: 20 },
      { name: 'Product 3', price: 30 },
      { name: 'Product 4', price: 40 },
      { name: 'Product 5', price: 50 },
      { name: 'Product 6', price: 60 },
    ],
  };

  render() {
    return (
      <div>
        {this.state.productList.map((product) => (
          <div className="product-card" key={product.name}>
            {product.name} - ${product.price}
            <button onClick={() => this.props.addToCart(product)}>
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    );
  }
}

export default ProductList;
```

### **Key Patterns Explained**

#### Pattern 1: Class Component State
```jsx
// INITIALIZE
class MyComponent extends Component {
  state = {
    count: 0,
    items: []
  };
  
  // UPDATE STATE
  incrementCount = () => {
    this.setState({ count: this.state.count + 1 });
  };
  
  // FUNCTIONAL UPDATE (when new state depends on old)
  addItem = (item) => {
    this.setState(prevState => ({
      items: [...prevState.items, item]
    }));
  };
}
```

#### Pattern 2: Array Methods for State
```jsx
// .some() - Check if any element matches condition
const hasApple = fruits.some(fruit => fruit === 'apple'); // true/false

// Spread operator - Add to array
const newArray = [...oldArray, newItem];

// Filter - Remove from array
const updated = items.filter(item => item.id !== idToRemove);
```

#### Pattern 3: Passing Methods as Props
```jsx
// PARENT
class Parent extends Component {
  handleClick = (data) => {
    console.log(data);
  };
  
  render() {
    return <Child onAction={this.handleClick} />;
  }
}

// CHILD
class Child extends Component {
  render() {
    return (
      <button onClick={() => this.props.onAction('Hello')}>
        Click me
      </button>
    );
  }
}
```

---

## 🔥 QUESTION 6: TIMER (Lifecycle Methods)

### **Core Concepts**
- Class component lifecycle
- `componentDidMount()` - runs after component loads
- `componentWillUnmount()` - runs before component is removed
- `setInterval()` - JavaScript timer function
- Cleanup to prevent memory leaks

### **Complete Solution: Timer.jsx**
```jsx
import React, { Component } from "react";

class Timer extends Component {
  // STEP 1: Initialize state and timer reference
  constructor(props) {
    super(props);
    this.state = { seconds: 0 };
    this.timer = null; // Will hold interval ID
  }

  // STEP 2: Start timer when component mounts
  componentDidMount() {
    // Set up interval (runs every 1000ms = 1 second)
    this.timer = setInterval(() => {
      // Update state using functional form
      this.setState(prevState => ({
        seconds: prevState.seconds + 1
      }));
    }, 1000);
  }

  // STEP 3: Clean up when component unmounts
  componentWillUnmount() {
    // Stop the interval to prevent memory leaks
    clearInterval(this.timer);
  }

  // STEP 4: Render current time
  render() {
    return (
      <div>
        <h2>Elapsed time: {this.state.seconds} seconds</h2>
      </div>
    );
  }
}

export default Timer;
```

### **Lifecycle Methods Explained**

#### Complete Lifecycle Flow
```jsx
class LifecycleDemo extends Component {
  constructor(props) {
    // 1. Component is created
    // Initialize state, bind methods
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    // 2. Component inserted into DOM
    // Perfect for: API calls, subscriptions, timers
    console.log("Mounted!");
  }

  componentDidUpdate(prevProps, prevState) {
    // 3. Component re-rendered (state/props changed)
    // Perfect for: responding to changes
    if (prevState.count !== this.state.count) {
      console.log("Count changed!");
    }
  }

  componentWillUnmount() {
    // 4. Component about to be removed
    // Perfect for: cleanup (stop timers, cancel requests)
    console.log("Unmounting...");
  }

  render() {
    // Called every time component updates
    return <div>{this.state.count}</div>;
  }
}
```

### **Functional Component Equivalent (useEffect)**
```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Runs after mount (like componentDidMount)
    const timer = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // Cleanup function (like componentWillUnmount)
    return () => {
      clearInterval(timer);
    };
  }, []); // Empty array = run once on mount

  return <h2>Elapsed time: {seconds} seconds</h2>;
}
```

### **Common Patterns**

#### Pattern 1: API Call on Mount
```jsx
componentDidMount() {
  fetch('https://api.example.com/data')
    .then(res => res.json())
    .then(data => this.setState({ data }));
}
```

#### Pattern 2: Subscribe to Events
```jsx
componentDidMount() {
  window.addEventListener('resize', this.handleResize);
}

componentWillUnmount() {
  window.removeEventListener('resize', this.handleResize);
}
```

#### Pattern 3: Update on Props Change
```jsx
componentDidUpdate(prevProps) {
  if (prevProps.userId !== this.props.userId) {
    this.fetchUser(this.props.userId);
  }
}
```

---

## 🔥 QUESTION 7: HERO MANAGER (Context API + useReducer)

### **Core Concepts**
- Context API (global state)
- `useReducer` (complex state logic)
- Custom hooks
- Multiple components sharing state

### **Architecture Overview**
```
App
├── HeroProvider (Context wrapper)
│   ├── HeroForm (add heroes)
│   ├── FilterBar (filter view)
│   ├── HeroStats (display counts)
│   └── HeroList
│       └── HeroCard (individual hero)
```

### **Solution 1: useHeroManager.js (State Logic)**
```js
import { useReducer } from "react";

// INITIAL STATE
const initialState = {
  heroes: [],
  filter: "All", // All | Active | Retired
};

// REDUCER FUNCTION (handles all state changes)
function heroReducer(state, action) {
  switch (action.type) {
    case "ADD_HERO":
      return {
        ...state,
        heroes: [
          ...state.heroes,
          {
            id: Date.now(), // Unique ID
            name: action.payload.name,
            power: action.payload.power,
            retired: false,
            onMission: false,
          },
        ],
      };

    case "DELETE_HERO":
      return {
        ...state,
        heroes: state.heroes.filter(
          (hero) => hero.id !== action.payload.id
        ),
      };

    case "TOGGLE_STATUS":
      return {
        ...state,
        heroes: state.heroes.map((hero) =>
          hero.id === action.payload.id
            ? { ...hero, retired: !hero.retired }
            : hero
        ),
      };

    case "ASSIGN_MISSION":
      return {
        ...state,
        heroes: state.heroes.map((hero) =>
          hero.id === action.payload.id
            ? { ...hero, onMission: true }
            : hero
        ),
      };

    case "REMOVE_MISSION":
      return {
        ...state,
        heroes: state.heroes.map((hero) =>
          hero.id === action.payload.id
            ? { ...hero, onMission: false }
            : hero
        ),
      };

    case "SET_FILTER":
      return {
        ...state,
        filter: action.payload,
      };

    default:
      return state;
  }
}

// CUSTOM HOOK (wraps useReducer and provides helper functions)
export function useHeroManager() {
  const [state, dispatch] = useReducer(heroReducer, initialState);

  // Helper functions (dispatch actions)
  const addHero = (name, power) =>
    dispatch({ type: "ADD_HERO", payload: { name, power } });

  const deleteHero = (id) =>
    dispatch({ type: "DELETE_HERO", payload: { id } });

  const toggleStatus = (id) =>
    dispatch({ type: "TOGGLE_STATUS", payload: { id } });

  const assignMission = (id) =>
    dispatch({ type: "ASSIGN_MISSION", payload: { id } });

  const removeMission = (id) =>
    dispatch({ type: "REMOVE_MISSION", payload: { id } });

  const setFilter = (filter) =>
    dispatch({ type: "SET_FILTER", payload: filter });

  return {
    heroes: state.heroes,
    filter: state.filter,
    addHero,
    deleteHero,
    toggleStatus,
    assignMission,
    removeMission,
    setFilter,
  };
}
```

### **Solution 2: HeroContext.js (Context Setup)**
```js
import React, { createContext } from "react";
import { useHeroManager } from "../hooks/useHeroManager";

// Create context
export const HeroContext = createContext();

// Provider component
export function HeroProvider({ children }) {
  const value = useHeroManager(); // Get all state and functions
  
  return (
    <HeroContext.Provider value={value}>
      {children}
    </HeroContext.Provider>
  );
}
```

### **Solution 3: HeroForm.js (Add Heroes)**
```jsx
import React, { useContext, useState } from "react";
import { HeroContext } from "../context/HeroContext";

function HeroForm() {
  const { addHero } = useContext(HeroContext);
  const [name, setName] = useState("");
  const [power, setPower] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (name && power) {
      addHero(name, power);
      setName("");
      setPower("");
    }
  };

  return (
    <form onSubmit={handle
