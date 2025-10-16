React Development Questions - Complete Documentation
PHASE-1: CONTEXT API, REF, HOC
Question No: 1 - ExpenseTracker_React
Project Type Question
Ram is a React developer, he is tasked with creating an expense tracker that will track all his expenses with the product name, and the amount spent. The application should add the expense upon submission. In addition to this, the application should not accept empty submissions, the application should update the existing expense if a duplicate expense is entered, and also the amount should not be negative. Help him achieve this by using react life cycle component methods.
Ram is a React developer, he is tasked with creating an expense tracker that will track all his expenses with the product name, and the amount spent. The application should add the expense upon submission. In addition, the application should not accept empty submissions, the application should update the existing expense if a duplicate expense is entered, and the amount should not be negative. Help him achieve this by using react life cycle component methods.
Requirements:

Create a component 'ExpenseTracker.jsx' inside 'src/components' that will be responsible for maintaining the list of products, their expenses, and the total cost.
Create a component 'ExpenseForm.jsx' inside 'src/components' that will collect the product name and the amount spent from the user. The collected data should be passed to ExpenseTracker.jsx.
Create a component 'ExpenseList.jsx' inside 'src/components' that will be responsible for rendering all the expenses as individual items.
All these components should be class components that will exhibit the component and should update the life cycle method.

Initial render:
Adding an expense:
Adding a duplicate expense:
Adding a negative expense:
Submitting an empty form:
To run the project:
Navigate to the reactapp directory and install all the node modules using npm install command then start the application using the npm start command.
To preview the output click on the 8081 port link.
Note: The placeholder of the input elements should exactly be the same, and the expenses should be rendered as unordered list items.
Answer:
File Structure
reactapp/
├── src/
│   ├── components/
│   │   ├── ExpenseTracker.jsx
│   │   ├── ExpenseForm.jsx
│   │   └── ExpenseList.jsx
│   ├── App.js
│   └── index.js
Code Implementation
src/components/ExpenseForm.jsx
jsximport React, { Component } from 'react';

class ExpenseForm extends Component {
  constructor(props) {
    super(props);
    this.state = {
      productName: '',
      amount: ''
    };
  }

  handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value
    });
  };

  handleSubmit = (e) => {
    e.preventDefault();
    const { productName, amount } = this.state;
    
    // Validation
    if (!productName.trim() || !amount.trim()) {
      alert('Please fill in all fields');
      return;
    }
    
    if (parseFloat(amount) < 0) {
      alert('Amount cannot be negative');
      return;
    }
    
    this.props.onAddExpense(productName, parseFloat(amount));
    this.setState({ productName: '', amount: '' });
  };

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <div>
          <input
            type="text"
            name="productName"
            placeholder="Product Name"
            value={this.state.productName}
            onChange={this.handleChange}
          />
        </div>
        <div>
          <input
            type="number"
            name="amount"
            placeholder="Amount"
            value={this.state.amount}
            onChange={this.handleChange}
          />
        </div>
        <button type="submit">Add Expense</button>
      </form>
    );
  }
}

export default ExpenseForm;
src/components/ExpenseList.jsx
jsximport React, { Component } from 'react';

class ExpenseList extends Component {
  render() {
    const { expenses } = this.props;
    
    return (
      <div>
        <h2>Expenses</h2>
        <ul>
          {expenses.map((expense, index) => (
            <li key={index}>
              {expense.productName}: ${expense.amount.toFixed(2)}
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

export default ExpenseList;
src/components/ExpenseTracker.jsx
jsximport React, { Component } from 'react';
import ExpenseForm from './ExpenseForm';
import ExpenseList from './ExpenseList';

class ExpenseTracker extends Component {
  constructor(props) {
    super(props);
    this.state = {
      expenses: [],
      totalCost: 0
    };
  }

  componentDidMount() {
    console.log('ExpenseTracker mounted');
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.expenses !== this.state.expenses) {
      console.log('Expenses updated');
    }
  }

  handleAddExpense = (productName, amount) => {
    const { expenses } = this.state;
    
    // Check for duplicate
    const existingIndex = expenses.findIndex(
      expense => expense.productName.toLowerCase() === productName.toLowerCase()
    );
    
    if (existingIndex !== -1) {
      // Update existing expense
      const updatedExpenses = [...expenses];
      updatedExpenses[existingIndex].amount = amount;
      
      this.setState({
        expenses: updatedExpenses,
        totalCost: updatedExpenses.reduce((sum, exp) => sum + exp.amount, 0)
      });
    } else {
      // Add new expense
      const newExpense = { productName, amount };
      const updatedExpenses = [...expenses, newExpense];
      
      this.setState({
        expenses: updatedExpenses,
        totalCost: updatedExpenses.reduce((sum, exp) => sum + exp.amount, 0)
      });
    }
  };

  render() {
    return (
      <div>
        <h1>Expense Tracker</h1>
        <ExpenseForm onAddExpense={this.handleAddExpense} />
        <ExpenseList expenses={this.state.expenses} />
        <h3>Total Cost: ${this.state.totalCost.toFixed(2)}</h3>
      </div>
    );
  }
}

export default ExpenseTracker;
src/App.js
jsximport React from 'react';
import ExpenseTracker from './components/ExpenseTracker';
import './App.css';

function App() {
  return (
    <div className="App">
      <ExpenseTracker />
    </div>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

Question No: 1 - Higher Order Component

**Project Type Question**

**1.Create WithLoading component**

Inside `src/components` directory create a file named `WithLoading.jsx`.
- Define the WithLoading function, which takes a WrappedComponent as a parameter.
- Inside the WithLoading function, return a new class component that extends Component. Initialise the state with isLoading set to true.
- Add the componentDidMount lifecycle method. Use setTimeout to simulate a loading delay and then update the state to set isLoading to false.
- In the render method, destructure isLoading from the state. Render either a loading message or the WrappedComponent based on the value of isLoading.

**2.Create MyComponent component**

Inside `src/components` directory create a file named `MyComponent.jsx`.
- Return heading(`<h1>`) with the content "Hello, I'm the wrapped component!".

**3.In `App.js` file**

Use the WithLoading higher-order component to wrap MyComponent. Create a new variable, for example, WrappedComponentWithLoading, and set it equal to the result of invoking WithLoading with MyComponent as an argument.

**4.Navigate to the React App Folder**

Open your terminal and change your working directory to the React app folder named `reactapp`.

**5.Install Dependencies**

Run the following command to install the necessary dependencies: `npm install`, `npm install canvas`

**6.Start the Development Server**

After the installation is complete, start the React development server with the following command:`npm start`

View the Output by clicking on above link `port: 8081`.

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── WithLoading.jsx
│   │   └── MyComponent.jsx
│   ├── App.js
│   └── index.js
Code Implementation
src/components/WithLoading.jsx
jsximport React, { Component } from 'react';

const WithLoading = (WrappedComponent) => {
  return class extends Component {
    constructor(props) {
      super(props);
      this.state = {
        isLoading: true
      };
    }

    componentDidMount() {
      setTimeout(() => {
        this.setState({ isLoading: false });
      }, 2000);
    }

    render() {
      const { isLoading } = this.state;
      
      if (isLoading) {
        return <div>Loading...</div>;
      }
      
      return <WrappedComponent {...this.props} />;
    }
  };
};

export default WithLoading;
src/components/MyComponent.jsx
jsximport React from 'react';

const MyComponent = () => {
  return <h1>Hello, I'm the wrapped component!</h1>;
};

export default MyComponent;
src/App.js
jsximport React from 'react';
import WithLoading from './components/WithLoading';
import MyComponent from './components/MyComponent';
import './App.css';

const WrappedComponentWithLoading = WithLoading(MyComponent);

function App() {
  return (
    <div className="App">
      <WrappedComponentWithLoading />
    </div>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install
npm install canvas

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - Superhero Team Manager

**Project Type Question**

**Problem Title: Superhero Team Manager**

**Scenario / Problem Statement**

You are assigned to build a superhero management application using React.js. The application should allow users to maintain a list of superheroes with the ability to:
- Add new heroes with a name and superpower.
- Assign or remove heroes from missions.
- Retire or reactivate heroes.
- Filter heroes based on their status (All, Active, Retired).
- Display real-time statistics about the team, including counts of active, retired, and mission-assigned heroes.

The app must use React Context and a custom hook (useHeroManager) to manage the global hero data, and it should be designed with modular components.

**Tech Stack:**
- Frontend: React.js (running on port 8081)

**Features to Implement:**

**1. Add Hero**
- Inputs for hero name and superpower.
- Button to submit and add the hero to the list.

**2. Hero List Display**
- Shows all heroes, filtered by status.
- Each hero has buttons for:
- Retire/Reactivate
- Assign/Remove from mission
- Delete

**3. Filtering Options**
- Buttons or dropdown to select:
- All
- Active
- Retired

**4. Hero Stats Dashboard**
- Shows:
- Total heroes
- Number of active heroes
- Number of retired heroes
- Number currently on missions

**5. State Management**
- Custom hook useHeroManager using useReducer.
- Context provider to share data across components.

**Folder Structure:**

**Component Overview**

**App.js**
- This is the main entry point for the application UI.

**Responsibilities:**
- Wraps everything in <HeroProvider> to give all components access to shared hero data via context.
- Renders the main layout:
- HeroForm (to add new heroes)
- FilterBar (to choose hero status view)
- HeroStats (dashboard-style stats summary)
- HeroList (to display heroes)

**Analogy:**
Think of this as the control center UI that connects and shows all other systems (components).

**HeroForm.js**

This is the form to add a new superhero.

**Features:**
- Inputs for Hero Name and Superpower.
- On form submit:
- Validates that both fields are filled.
- Calls addHero() from context to add a new hero to global state.
- Resets form fields after submission.

**HeroList.js**

Displays the list of all superheroes, filtered by the current status (All, Active, Retired).

**Features:**
- Gets heroes and filter from context.
- Filters heroes accordingly:
- If "All", show all.
- If "Active", only non-retired.
- If "Retired", only retired.
- Maps each hero to a HeroCard.

**HeroCard.js**

Represents one individual hero card in the list.

**Displays:**
- Hero's name, power, and status (Active/Retired)
- Action buttons:
- Toggle Retirement — mark hero as retired or active.
- Assign/Remove Mission — manage mission status.
- Delete — remove hero.

**FilterBar.js**

Allows users to filter the list of heroes.

**Features:**
- Buttons or dropdown for:
- All heroes
- Active heroes
- Retired heroes
- Calls setFilter() from context to change the view.

**HeroStats.js**

Displays real-time stats about heroes in the system.

**Metrics shown:**
- Total number of heroes
- Number of active heroes
- Number of retired heroes
- How many are currently on missions

**useHeroManager.js (Custom Hook)**

A custom React hook that manages all hero-related state and logic using useReducer.

**Key things it manages:**
- Hero list state (heroes)
- Adding, deleting, updating heroes
- Toggling retirement
- Assigning or removing missions
- Setting the current filter

**HeroContext.js**

Provides a React Context to share the hero state across all components.

**Responsibilities:**
- Wraps the custom hook (useHeroManager)
- Creates a HeroContext.Provider that wraps the app
- Any component can useContext(HeroContext) to access shared state and functions

**Sample Output:**

**Steps to run the Application :**

Open the terminal and follow the steps below:

**Step 1:** Use "cd reactapp" command to navigate to the "reactapp" directory.

**Step 2:** Use "npm i" command to install the node_modules.

**Step 3:** Use "npm start" command to start the application.

**Note :**
- Click "PORT: 8081" to view the output.
- Click "Run Test Case" button to run the testcases.
- If any error persists while running the app, delete the node_modules and reinstall them.

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── HeroForm.js
│   │   ├── HeroList.js
│   │   ├── HeroCard.js
│   │   ├── FilterBar.js
│   │   └── HeroStats.js
│   ├── context/
│   │   └── HeroContext.js
│   ├── hooks/
│   │   └── useHeroManager.js
│   ├── App.js
│   └── index.js
Code Implementation
src/hooks/useHeroManager.js
jsximport { useReducer, useState } from 'react';

const heroReducer = (state, action) => {
  switch (action.type) {
    case 'ADD_HERO':
      return [...state, {
        id: Date.now(),
        name: action.payload.name,
        power: action.payload.power,
        retired: false,
        onMission: false
      }];
    
    case 'DELETE_HERO':
      return state.filter(hero => hero.id !== action.payload);
    
    case 'TOGGLE_RETIREMENT':
      return state.map(hero =>
        hero.id === action.payload
          ? { ...hero, retired: !hero.retired }
          : hero
      );
    
    case 'TOGGLE_MISSION':
      return state.map(hero =>
        hero.id === action.payload
          ? { ...hero, onMission: !hero.onMission }
          : hero
      );
    
    default:
      return state;
  }
};

const useHeroManager = () => {
  const [heroes, dispatch] = useReducer(heroReducer, []);
  const [filter, setFilter] = useState('All');

  const addHero = (name, power) => {
    dispatch({ type: 'ADD_HERO', payload: { name, power } });
  };

  const deleteHero = (id) => {
    dispatch({ type: 'DELETE_HERO', payload: id });
  };

  const toggleRetirement = (id) => {
    dispatch({ type: 'TOGGLE_RETIREMENT', payload: id });
  };

  const toggleMission = (id) => {
    dispatch({ type: 'TOGGLE_MISSION', payload: id });
  };

  return {
    heroes,
    filter,
    setFilter,
    addHero,
    deleteHero,
    toggleRetirement,
    toggleMission
  };
};

export default useHeroManager;
src/context/HeroContext.js
jsximport React, { createContext } from 'react';
import useHeroManager from '../hooks/useHeroManager';

export const HeroContext = createContext({
  heroes: [],
  addHero: () => {},
  deleteHero: () => {},
  toggleRetirement: () => {},
  toggleMission: () => {},
  filter: 'All',
  setFilter: () => {}
});

export const HeroProvider = ({ children }) => {
  const heroManager = useHeroManager();

  return (
    <HeroContext.Provider value={heroManager}>
      {children}
    </HeroContext.Provider>
  );
};
src/components/HeroForm.js
jsximport React, { useState, useContext } from 'react';
import { HeroContext } from '../context/HeroContext';

const HeroForm = () => {
  const [name, setName] = useState('');
  const [power, setPower] = useState('');
  const { addHero } = useContext(HeroContext);

  const handleSubmit = (e) => {
    e.preventDefault();
    if (name.trim() && power.trim()) {
      addHero(name, power);
      setName('');
      setPower('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Hero Name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="text"
        placeholder="Superpower"
        value={power}
        onChange={(e) => setPower(e.target.value)}
      />
      <button type="submit">Add Hero</button>
    </form>
  );
};

export default HeroForm;
src/components/HeroCard.js
jsximport React, { useContext } from 'react';
import { HeroContext } from '../context/HeroContext';

const HeroCard = ({ hero }) => {
  const { deleteHero, toggleRetirement, toggleMission } = useContext(HeroContext);

  return (
    <div data-testid="hero-card" style={{ border: '1px solid #ccc', padding: '10px', margin: '10px' }}>
      <h3>{hero.name}</h3>
      <p>Power: {hero.power}</p>
      <p>Status: {hero.retired ? 'Retired' : 'Active'}</p>
      <p>Mission: {hero.onMission ? 'On Mission' : 'Available'}</p>
      <button onClick={() => toggleRetirement(hero.id)}>
        {hero.retired ? 'Reactivate' : 'Retire'}
      </button>
      <button onClick={() => toggleMission(hero.id)}>
        {hero.onMission ? 'Remove from Mission' : 'Assign Mission'}
      </button>
      <button onClick={() => deleteHero(hero.id)}>Delete</button>
    </div>
  );
};

export default HeroCard;
src/components/HeroList.js
jsximport React, { useContext } from 'react';
import { HeroContext } from '../context/HeroContext';
import HeroCard from './HeroCard';

const HeroList = () => {
  const { heroes, filter } = useContext(HeroContext);

  const filteredHeroes = heroes.filter(hero => {
    if (filter === 'All') return true;
    if (filter === 'Active') return !hero.retired;
    if (filter === 'Retired') return hero.retired;
    return true;
  });

  return (
    <div className="hero-list">
      {filteredHeroes.map(hero => (
        <HeroCard key={hero.id} hero={hero} />
      ))}
    </div>
  );
};

export default HeroList;
src/components/FilterBar.js
jsximport React, { useContext } from 'react';
import { HeroContext } from '../context/HeroContext';

const FilterBar = () => {
  const { filter, setFilter } = useContext(HeroContext);

  return (
    <div>
      <button onClick={() => setFilter('All')}>All</button>
      <button onClick={() => setFilter('Active')}>Active</button>
      <button onClick={() => setFilter('Retired')}>Retired</button>
    </div>
  );
};

export default FilterBar;
src/components/HeroStats.js
jsximport React, { useContext } from 'react';
import { HeroContext } from '../context/HeroContext';

const HeroStats = () => {
  const { heroes } = useContext(HeroContext);

  const totalHeroes = heroes.length;
  const activeHeroes = heroes.filter(h => !h.retired).length;
  const retiredHeroes = heroes.filter(h => h.retired).length;
  const onMissionHeroes = heroes.filter(h => h.onMission).length;

  return (
    <div>
      <h2>Hero Statistics</h2>
      <p>Total Heroes: {totalHeroes}</p>
      <p>Active Heroes: {activeHeroes}</p>
      <p>Retired Heroes: {retiredHeroes}</p>
      <p>Heroes on Mission: {onMissionHeroes}</p>
    </div>
  );
};

export default HeroStats;
src/App.js
jsximport React from 'react';
import { HeroProvider } from './context/HeroContext';
import HeroForm from './components/HeroForm';
import FilterBar from './components/FilterBar';
import HeroStats from './components/HeroStats';
import HeroList from './components/HeroList';

function App() {
  return (
    <HeroProvider>
      <div className="App">
        <h1>Superhero Operations Center</h1>
        <HeroForm />
        <FilterBar />
        <HeroStats />
        <HeroList />
      </div>
    </HeroProvider>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm i

# Start the application
npm start

# Click on PORT: 8081 to view output
# Click "Run Test Case" to run tests
```

---

## PHASE-2: ROUTING AND LAZY LOADING

### Question No: 1 - Browser Routing

**Project Type Question**

**Instructions for Setting Up and Running the React App:**

**1.Create the Navbar Component**

Inside the `src/components` folder, create a new file named `Navbar.jsx`

This component includes a navigation bar with links to the home and about pages, styled using the imported CSS.
- Add the (`<div>`) tag with the class name "navbar-container".
- Inside the <div>, create an unordered list (`<ul>`) with the class name "nav-links".
- Inside the list, create list items (`<li>`) for each navigation link.
- For each list item, add an anchor (`<Link>`) from react-router-dom with a class name (home or about) and the respective text content (Home or About).

**2.Create the Home Component**

Inside the `src/components` folder, create a new file named `Home.jsx`
- Create the Home component function. Inside the function, return a (`<div>`) element that wraps the content of your home page.
- Inside the (`<div>`), add an (`<h2>`) element with the text content "Home". This will serve as the heading of your home page.
- Below the heading, add a (`<p>`) element with the text content "Welcome to the home page!". This will be the introductory paragraph.

**3.Create the About Component**

Inside the `src/components` folder, create a new file named `About.jsx`
- Create the About component function. Inside the function, return a (`<div>`) element that wraps the content of your about page.
- Inside the (`<div>`), add an (`<h2>`) element with the text content "About". This will serve as the heading of your about page.
- Below the heading, add a (`<p>`) element with the text content "This is the about page.". This will be the introductory paragraph.

**4.Inside App Component**

Open `App.js`. Import React, styles, and necessary components (Home, About, Navbar, Route, Routes). In the App function, return a (`<div>`) with class name "App". Inside the div, render the Navbar component and use the Routes component to define two routes. One route should match the exact path '/' and render the Home component, and the other should match the path '/about' and render the About component.

**5.Inside Index Component**

Inside the root.render method, wrap your App component with React.StrictMode and BrowserRouter from react-router-dom. This provides additional development features and enables client-side routing.

**6.Navigate to the React App Folder**

Open your terminal and change your working directory to the React app folder named `reactapp`

**7.Install Dependencies**

Run the following command to install the necessary dependencies: `npm install`, `npm install canvas`

**8.Start the Development Server**

After the installation is complete, start the React development server with the following command:`npm start`

View the Output by clicking on above link `port: 8081`

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── Navbar.jsx
│   │   ├── Home.jsx
│   │   └── About.jsx
│   ├── App.js
│   └── index.js
Code Implementation
src/components/Navbar.jsx
jsximport React from 'react';
import { Link } from 'react-router-dom';

const Navbar = () => {
  return (
    <div className="navbar-container">
      <ul className="nav-links">
        <li>
          <Link to="/" className="home">Home</Link>
        </li>
        <li>
          <Link to="/about" className="about">About</Link>
        </li>
      </ul>
    </div>
  );
};

export default Navbar;
src/components/Home.jsx
jsximport React from 'react';

const Home = () => {
  return (
    <div>
      <h2>Home</h2>
      <p>Welcome to the home page!</p>
    </div>
  );
};

export default Home;
src/components/About.jsx
jsximport React from 'react';

const About = () => {
  return (
    <div>
      <h2>About</h2>
      <p>This is the about page.</p>
    </div>
  );
};

export default About;
src/App.js
jsximport React from 'react';
import { Routes, Route } from 'react-router-dom';
import Navbar from './components/Navbar';
import Home from './components/Home';
import About from './components/About';
import './App.css';

function App() {
  return (
    <div className="App">
      <Navbar />
      <Routes>
        <Route exact path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </div>
  );
}

export default App;
src/index.js
jsximport React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install
npm install canvas

# Start the application
npm start

# Click on PORT: 8081 to view output

Question No: 1 - SPA with Code Splitting and Lazy Loading
Project Type Question
Scenario:
You have been tasked with improving the performance of a React Single Page Application (SPA) for a company's internal dashboard. The application has multiple features, including a dashboard with various modules (e.g., Charts, Reports, Admin Settings, etc.), user authentication, and a global navigation system.
Your goal is to implement code splitting and lazy loading throughout the application to reduce the initial page load time, making the app faster and more user-friendly.
You will implement React.lazy(), Suspense, and Webpack optimization techniques to achieve this.
Features:

Code Splitting with React.lazy
Routing (React Router)
Suspense + Fallback UI
Error Boundaries
Dynamic Imports (on interaction)

Tasks:

Setup the SPA project with React, Webpack, and necessary loaders.
Split your application into logical sections (routes, components, etc.) for lazy loading:
Lazy load major routes like Dashboard, Reports, Settings, and User Profile.
Break down large components like Charts, Modals, and Tables into smaller lazy-loaded chunks.

Implement Suspense with Fallbacks:
Use the Suspense component with fallback loading indicators like spinners or skeleton loaders for each lazy-loaded route/component.
Create a global fallback for the entire application and a custom fallback for individual components (e.g., for slow network conditions).
Optimize with Webpack's splitChunks:
Configure Webpack to optimize the bundling of shared dependencies and ensure that they are loaded only once.
Ensure vendor code (third-party libraries) like react-router-dom, axios, react-redux, etc., is extracted into separate chunks.
Implement Error Boundaries:
Use React Error Boundaries around lazy-loaded components to catch loading errors (e.g., chunk loading failures) and provide a meaningful error message or fallback UI.
Preload important resources:

Use webpackPrefetch or webpackPreload for routes or components that the user is likely to visit next, especially after authentication.
Test the lazy loading of these routes and components under slow network conditions using Chrome DevTools' network throttling.
Monitor bundle sizes with tools like webpack-bundle-analyzer and verify performance improvements using Lighthouse and React Profiler.
Use dynamic imports to load modals, charts, and large forms only when necessary (e.g., when a user clicks a button to show the modal or chart).
Write unit and integration tests for lazy-loaded components:
Mock lazy-loaded components during unit testing with Jest.
Write integration tests to ensure the fallback is properly rendered and the actual component loads after the chunk is fetched.
Answer:
This is a comprehensive scenario-based question requiring implementation of multiple advanced React patterns. Below is a complete implementation guide:
File Structure
reactapp/
├── src/
│   ├── components/
│   │   ├── ErrorBoundary.js
│   │   ├── LoadingFallback.js
│   │   └── Navigation.js
│   ├── pages/
│   │   ├── Dashboard.js
│   │   ├── Reports.js
│   │   ├── Settings.js
│   │   ├── UserProfile.js
│   │   └── AdminDashboard.js
│   ├── App.js
│   ├── index.js
│   └── webpack.config.js
Code Implementation
src/components/ErrorBoundary.js
jsximport React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div style={{ padding: '20px', textAlign: 'center' }}>
          <h2>Something went wrong loading this component</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
src/components/LoadingFallback.js
jsximport React from 'react';

const LoadingFallback = () => {
  return (
    <div style={{ 
      display: 'flex', 
      justifyContent: 'center', 
      alignItems: 'center', 
      height: '100vh' 
    }}>
      <div className="spinner">Loading...</div>
    </div>
  );
};

export default LoadingFallback;
src/components/Navigation.js
jsximport React from 'react';
import { Link } from 'react-router-dom';

const Navigation = () => {
  return (
    <nav style={{ padding: '10px', background: '#333', color: 'white' }}>
      <Link to="/" style={{ margin: '0 10px', color: 'white' }}>Dashboard</Link>
      <Link to="/reports" style={{ margin: '0 10px', color: 'white' }}>Reports</Link>
      <Link to="/settings" style={{ margin: '0 10px', color: 'white' }}>Settings</Link>
      <Link to="/profile" style={{ margin: '0 10px', color: 'white' }}>Profile</Link>
      <Link to="/admin" style={{ margin: '0 10px', color: 'white' }}>Admin</Link>
    </nav>
  );
};

export default Navigation;
src/pages/Dashboard.js
jsximport React from 'react';

const Dashboard = () => {
  return (
    <div>
      <h1>Dashboard</h1>
      <p>Welcome to the dashboard with charts and analytics</p>
    </div>
  );
};

export default Dashboard;
src/pages/Reports.js
jsximport React from 'react';

const Reports = () => {
  return (
    <div>
      <h1>Reports</h1>
      <p>View your reports here</p>
    </div>
  );
};

export default Reports;
src/pages/Settings.js
jsximport React from 'react';

const Settings = () => {
  return (
    <div>
      <h1>Settings</h1>
      <p>Manage your application settings</p>
    </div>
  );
};

export default Settings;
src/pages/UserProfile.js
jsximport React from 'react';

const UserProfile = () => {
  return (
    <div>
      <h1>User Profile</h1>
      <p>View and edit your profile</p>
    </div>
  );
};

export default UserProfile;
src/pages/AdminDashboard.js
jsximport React from 'react';

const AdminDashboard = () => {
  return (
    <div>
      <h1>Admin Dashboard</h1>
      <p>Administrative controls and settings</p>
    </div>
  );
};

export default AdminDashboard;
src/App.js
jsximport React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Navigation from './components/Navigation';
import LoadingFallback from './components/LoadingFallback';
import ErrorBoundary from './components/ErrorBoundary';

// Lazy load all pages with webpackPrefetch for likely next routes
const Dashboard = lazy(() => import(/* webpackPrefetch: true */ './pages/Dashboard'));
const Reports = lazy(() => import(/* webpackPrefetch: true */ './pages/Reports'));
const Settings = lazy(() => import('./pages/Settings'));
const UserProfile = lazy(() => import('./pages/UserProfile'));
const AdminDashboard = lazy(() => import(/* webpackPreload: true */ './pages/AdminDashboard'));

function App() {
  return (
    <Router>
      <div className="App">
        <Navigation />
        <ErrorBoundary>
          <Suspense fallback={<LoadingFallback />}>
            <Routes>
              <Route path="/" element={<Dashboard />} />
              <Route path="/reports" element={<Reports />} />
              <Route path="/settings" element={<Settings />} />
              <Route path="/profile" element={<UserProfile />} />
              <Route path="/admin" element={<AdminDashboard />} />
            </Routes>
          </Suspense>
        </ErrorBoundary>
      </div>
    </Router>
  );
}

export default App;
src/webpack.config.js
javascriptconst path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js',
    chunkFilename: '[name].[contenthash].chunk.js'
  },
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        },
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true
        }
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-react']
          }
        }
      }
    ]
  }
};
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - E-Commerce SPA with Code Splitting

**Project Type Question**

**Hands-on Project: Implementing Code Splitting in a Real SPA**

**Problem Statement:**

You are developing a Single Page Application (SPA) for an e-commerce website that contains multiple sections like Home, Products, Cart, Checkout, User Profile, and Admin Dashboard. The app's JavaScript bundle is growing large due to many dependencies and components. This causes slow load times, especially for users on mobile devices with slower internet connections.

To improve the performance and load time of the app, you need to implement code splitting so that only the essential parts of the app are loaded at the beginning, and other parts are loaded as needed (on demand).

**Your Task:**

**Initial Setup:**

You are using React with React Router for routing and Webpack as your bundler.

Implement code splitting to load parts of the application on demand. For instance, when the user navigates from the home page to the product listing or admin dashboard, only the necessary JavaScript for that route should be loaded.

**Implementing Code Splitting with React.lazy() and Suspense:**

Use React.lazy() to lazily load components for different sections of the app (e.g., Home, Products, Cart, etc.).

Use Suspense to display a loading spinner or placeholder while components are being fetched.

**Dynamic Import for Routes:**

Use React Router to set up routing in your app.

Split the routing for each section so that each page/component (Home, Products, Cart, etc.) has its own chunk loaded dynamically when the user navigates to it.

**Chunking Strategies:**

Set up Webpack to split the JavaScript bundle into smaller, logical chunks based on routes or features.

Use Webpack's splitChunks optimization to manage shared dependencies and ensure that third-party libraries (e.g., React, Lodash) are bundled separately for caching and faster load times.

**Optimize User Experience:**

Use Suspense to show a loading spinner while lazy-loaded components (like the Products page or Cart page) are being fetched.

Preload or prefetch some chunks (e.g., admin-related content or the product list) that might be needed soon after the user interacts with the app.

**Webpack Configuration for Code Splitting:**

Configure Webpack to split code intelligently, ensuring that chunks are created for each route and that shared dependencies are not bundled multiple times.

Implement code splitting for vendor chunks (third-party libraries) to avoid loading them unnecessarily across different routes.

**Output:**

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── Header.js
│   │   └── Loader.js
│   ├── pages/
│   │   ├── Home.js
│   │   ├── Products.js
│   │   ├── Cart.js
│   │   ├── Checkout.js
│   │   ├── UserProfile.js
│   │   └── AdminDashboard.js
│   ├── App.js
│   └── index.js
Code Implementation
src/components/Header.js
jsximport React from 'react';
import { Link } from 'react-router-dom';

const Header = () => {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/products">Products</Link>
      <Link to="/cart">Cart</Link>
      <Link to="/checkout">Checkout</Link>
      <Link to="/profile">Profile</Link>
      <Link to="/dashboard">Dashboard</Link>
    </nav>
  );
};

export default Header;
src/components/Loader.js
jsximport React from 'react';

const Loader = () => {
  return <div>loading</div>;
};

export default Loader;
src/pages/Home.js
jsximport React from 'react';

const Home = () => {
  return (
    <div>
      <p>Welcome to our E-Commerce Website</p>
    </div>
  );
};

export default Home;
src/pages/Products.js
jsximport React from 'react';

const Products = () => {
  return (
    <div>
      <p>Product List</p>
    </div>
  );
};

export default Products;
src/pages/Cart.js
jsximport React from 'react';

const Cart = () => {
  return (
    <div>
      <p>Your Shopping Cart</p>
    </div>
  );
};

export default Cart;
src/pages/Checkout.js
jsximport React from 'react';

const Checkout = () => {
  return (
    <div>
      <p>Checkout Page</p>
    </div>
  );
};

export default Checkout;
src/pages/UserProfile.js
jsximport React from 'react';

const UserProfile = () => {
  return (
    <div>
      <p>User Profile</p>
    </div>
  );
};

export default UserProfile;
src/pages/AdminDashboard.js
jsximport React from 'react';

const AdminDashboard = () => {
  return (
    <div>
      <p>Admin Dashboard</p>
    </div>
  );
};

export default AdminDashboard;
src/App.js
jsximport React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header';
import Loader from './components/Loader';

// Lazy load all pages
const Home = lazy(() => import('./pages/Home'));
const Products = lazy(() => import('./pages/Products'));
const Cart = lazy(() => import('./pages/Cart'));
const Checkout = lazy(() => import('./pages/Checkout'));
const UserProfile = lazy(() => import('./pages/UserProfile'));
const AdminDashboard = lazy(() => import('./pages/AdminDashboard'));

function App() {
  return (
    <Router>
      <Header />
      <Suspense fallback={<Loader />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/products" element={<Products />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/checkout" element={<Checkout />} />
          <Route path="/profile" element={<UserProfile />} />
          <Route path="/dashboard" element={<AdminDashboard />} />
        </Routes>
      </Suspense>
    </Router>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm i

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - Dashboard with Lazy Loading (6 Pages)

**Project Type Question**

**Problem Statement**

You are developing a React-based dashboard application that contains multiple pages such as Home, Profile, Settings, Reports, Notifications, and Help. The application is becoming large, resulting in slow initial load times. Your task is to implement lazy loading and code splitting to optimize performance by loading components only when required (on demand).

You must:

Split the code into chunks using React.lazy and React Router's dynamic import strategy.

Create a Loader fallback component to display while components are loading.

Organize the components in a modular file structure.

Demonstrate at least 6 different components/pages and lazy load each.

**Output:**

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   └── Loader.jsx
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Profile.jsx
│   │   ├── Reports.jsx
│   │   ├── Notifications.jsx
│   │   ├── Settings.jsx
│   │   └── Help.jsx
│   ├── routes/
│   │   └── AppRoutes.jsx
│   ├── App.js
│   └── index.js
Code Implementation
src/components/Loader.jsx
jsximport React from 'react';

const Loader = () => {
  return <div>Loading...</div>;
};

export default Loader;
src/pages/Home.jsx
jsximport React from 'react';

const Home = () => {
  return <h1>Welcome to Home Page</h1>;
};

export default Home;
src/pages/Profile.jsx
jsximport React from 'react';

const Profile = () => {
  return <h1>User Profile</h1>;
};

export default Profile;
src/pages/Reports.jsx
jsximport React from 'react';

const Reports = () => {
  return <h1>Analytics Reports</h1>;
};

export default Reports;
src/pages/Notifications.jsx
jsximport React from 'react';

const Notifications = () => {
  return <h1>Your Notifications</h1>;
};

export default Notifications;
src/pages/Settings.jsx
jsximport React from 'react';

const Settings = () => {
  return <h1>App Settings</h1>;
};

export default Settings;
src/pages/Help.jsx
jsximport React from 'react';

const Help = () => {
  return <h1>Help Center</h1>;
};

export default Help;
src/routes/AppRoutes.jsx
jsximport React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import Loader from '../components/Loader';

// Lazily import all the page components
const Home = lazy(() => import('../pages/Home'));
const Profile = lazy(() => import('../pages/Profile'));
const Reports = lazy(() => import('../pages/Reports'));
const Notifications = lazy(() => import('../pages/Notifications'));
const Settings = lazy(() => import('../pages/Settings'));
const Help = lazy(() => import('../pages/Help'));

const AppRoutes = () => {
  return (
    <Router>
      <div>
        <nav>
          <Link to="/" style={{ marginRight: '10px' }}>Home</Link>
          <Link to="/profile" style={{ marginRight: '10px' }}>Profile</Link>
          <Link to="/reports" style={{ marginRight: '10px' }}>Reports</Link>
          <Link to="/notifications" style={{ marginRight: '10px' }}>Notifications</Link>
          <Link to="/settings" style={{ marginRight: '10px' }}>Settings</Link>
          <Link to="/help" style={{ marginRight: '10px' }}>Help</Link>
        </nav>
        
        <Suspense fallback={<Loader />}>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/profile" element={<Profile />} />
            <Route path="/reports" element={<Reports />} />
            <Route path="/notifications" element={<Notifications />} />
            <Route path="/settings" element={<Settings />} />
            <Route path="/help" element={<Help />} />
          </Routes>
        </Suspense>
      </div>
    </Router>
  );
};

export default AppRoutes;
src/App.js
jsximport React from 'react';
import AppRoutes from './routes/AppRoutes';

function App() {
  return (
    <div className="App">
      <AppRoutes />
    </div>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm i

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - Code Splitting with Home and About

**Project Type Question**

**Problem Statement:**

**Code Splitting and Lazy Loading in React using Webpack**

You are building a React application with multiple components, including Home and About, to demonstrate the concept of code splitting using React.lazy() and Suspense. The main objective is to improve performance by splitting the app's code into smaller bundles that can be loaded on demand.

To achieve this, you will:

Use React.lazy() to lazily load components like Home and About.

Wrap lazy components in <Suspense> with a fallback loader.

Ensure routing using react-router-dom to navigate between components.

Webpack will automatically handle the code splitting at build time via dynamic imports.

**Expected Functional Behavior:**

When the user visits the root path (/), the Home component should be rendered.

When the user clicks on a navigation link like About, the About component should be loaded lazily and displayed.

While the lazy component is being loaded, a "Loading..." indicator should be shown.

**Test Case Objectives:**

You must write test cases that verify:

The loading fallback ("Loading...") appears while the component is being lazily loaded.

The Home component renders successfully after the lazy load.

The expected content (e.g., "Welcome to the Home Page") is present in the loaded component.

**Tools & Technologies:**

React

React Router v6

React.lazy() & Suspense

Webpack (for behind-the-scenes code splitting)

Jest & React Testing Library (for writing test cases)

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── Home.jsx
│   │   └── About.jsx
│   ├── App.js
│   └── index.js
Code Implementation
src/components/Home.jsx
jsximport React from 'react';

const Home = () => {
  return (
    <div>
      <h1>Welcome to the Home Page</h1>
    </div>
  );
};

export default Home;
src/components/About.jsx
jsximport React from 'react';

const About = () => {
  return (
    <div>
      <h1>About Us</h1>
    </div>
  );
};

export default About;
src/App.js
jsximport React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';

// Lazy loading components
const Home = lazy(() => import('./components/Home'));
const About = lazy(() => import('./components/About'));

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>

      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </Suspense>
    </Router>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

## PHASE-3: REDUX

### Question No: 1 - Counter App using Redux

**Project Type Question**

**Counter App using Redux**

**1.Create the Counter Component**

Inside the `src/components` folder, create a new file named `Counter.jsx`. Add the necessary code for the Counter component that renders the count in paragraph (`<p>`) with the content "Count: 0" and provides buttons for incrementing and decrementing.

**2.Create Redux Files**

Inside the `src/redux` folder, create three new files: `CounterAction.js`, `CounterReducer.js`, and `Store.js`.

**3.Use Provider in App Component**

Open the `src/App.js` file and add the necessary imports. Wrap the Counter component with the Provider from the react-redux library and pass the Redux store (`Store`) as a prop.

**4.Navigate to the React App Folder**

Open your terminal and change your working directory to the React app folder named `reactapp`

**5.Install Dependencies**

Run the following command to install the necessary dependencies: `npm install`, `npm install canvas`

**6.Start the Development Server**

After the installation is complete, start the React development server with the following command:`npm start`

View the output by clicking on the above link `port: 8081`

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   └── Counter.jsx
│   ├── redux/
│   │   ├── CounterAction.js
│   │   ├── CounterReducer.js
│   │   └── Store.js
│   ├── App.js
│   └── index.js
Code Implementation
src/redux/CounterAction.js
jsxexport const increment = () => ({
  type: 'INCREMENT'
});

export const decrement = () => ({
  type: 'DECREMENT'
});
src/redux/CounterReducer.js
jsxconst initialState = {
  count: 0
};

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;
src/redux/Store.js
jsximport { createStore } from 'redux';
import counterReducer from './CounterReducer';

const store = createStore(counterReducer);

export default store;
src/components/Counter.jsx
jsximport React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from '../redux/CounterAction';

const Counter = () => {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
};

export default Counter;
src/App.js
jsximport React from 'react';
import { Provider } from 'react-redux';
import store from './redux/Store';
import Counter from './components/Counter';

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <Counter />
      </div>
    </Provider>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install
npm install canvas

# Start the application
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - Stopwatch using Redux Toolkit

**Project Type Question**

**Stopwatch_ReactRedux**

You are tasked with creating a react application to store and display the stopwatch from redux. The stopwatch should contain the start, stop, reset, and save buttons. The list of old timers should be shown as an unordered list.

**Requirements:**

1. By default, the application should start a new timer with 00:00:00 as in Minutes: Seconds: Milliseconds.
2. The 'Start' button will initiate the stopwatch, the 'Stop' button will pause the timer, and the 'Reset' button will reset the timer.
3. The 'Save' button should trigger the application to store the stopwatch timer as it is to the redux, and all the stored timers will be displayed as an unordered list with a maximum of 5 with the recent being displayed on top.

**Note:** You should use the redux toolkit to implement this,

**Initial components:**

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   └── Stopwatch.jsx
│   ├── redux/
│   │   ├── stopwatchSlice.js
│   │   └── store.js
│   ├── App.js
│   └── index.js
Code Implementation
src/redux/stopwatchSlice.js
jsximport { createSlice } from '@reduxjs/toolkit';

const stopwatchSlice = createSlice({
  name: 'stopwatch',
  initialState: {
    time: 0,
    isRunning: false,
    savedTimes: []
  },
  reducers: {
    start: (state) => {
      state.isRunning = true;
    },
    stop: (state) => {
      state.isRunning = false;
    },
    reset: (state) => {
      state.time = 0;
      state.isRunning = false;
    },
    tick: (state) => {
      if (state.isRunning) {
        state.time += 10;
      }
    },
    save: (state) => {
      if (state.savedTimes.length >= 5) {
        state.savedTimes.pop();
      }
      state.savedTimes.unshift(state.time);
    }
  }
});

export const { start, stop, reset, tick, save } = stopwatchSlice.actions;
export default stopwatchSlice.reducer;
src/redux/store.js
jsximport { configureStore } from '@reduxjs/toolkit';
import stopwatchReducer from './stopwatchSlice';

const store = configureStore({
  reducer: {
    stopwatch: stopwatchReducer
  }
});

export default store;
src/components/Stopwatch.jsx
jsximport React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { start, stop, reset, tick, save } from '../redux/stopwatchSlice';

const Stopwatch = () => {
  const { time, isRunning, savedTimes } = useSelector(state => state.stopwatch);
  const dispatch = useDispatch();

  useEffect(() => {
    let interval = null;
    if (isRunning) {
      interval = setInterval(() => {
        dispatch(tick());
      }, 10);
    }
    return () => clearInterval(interval);
  }, [isRunning, dispatch]);

  const formatTime = (milliseconds) => {
    const ms = Math.floor((milliseconds % 1000) / 10);
    const seconds = Math.floor((milliseconds / 1000) % 60);
    const minutes = Math.floor((milliseconds / 60000) % 60);
    
    return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}:${ms.toString().padStart(2, '0')}`;
  };

  return (
    <div>
      <h1>{formatTime(time)}</h1>
      <button onClick={() => dispatch(start())}>Start</button>
      <button onClick={() => dispatch(stop())}>Stop</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
      <button onClick={() => dispatch(save())}>Save</button>
      
      <h2>Saved Times:</h2>
      <ul>
        {savedTimes.map((savedTime, index) => (
          <li key={index}>{formatTime(savedTime)}</li>
        ))}
      </ul>
    </div>
  );
};

export default Stopwatch;
src/App.js
jsximport React from 'react';
import { Provider } from 'react-redux';
import store from './redux/store';
import Stopwatch from './components/Stopwatch';

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <Stopwatch />
      </div>
    </Provider>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm i

# Start the application
npm start

# Click on PORT: 8081 to view output

Question No: 1 - Campaign Manager (Redux Toolkit)
Project Type Question
Project Title: Campaign Manager (React + Redux Toolkit)
Problem Statement / Scenario
You are tasked with building a Campaign Manager application that allows users to manage a list of campaigns. The system should:

Enable users to add new campaigns with a title.
Display all saved campaigns in a list.
Allow users to update/edit campaign titles.
Allow users to delete campaigns.
Manage campaigns globally using Redux Toolkit.
Be testable using React Testing Library for components and direct Redux tests for slice logic.
Key Functionalities
1. CampaignForm

Input field for campaign title.
Add button to submit and save a new campaign.
Validates that the title is not empty.
Resets input after submission.

2. CampaignList

Displays all campaigns in a list.
Each campaign item has:
Input field to edit the title.
Delete button to remove the campaign.
Updates should reflect immediately in the displayed list.

3. Redux Slice
State: campaigns array. Each campaign object contains:
{
id: 'uuid', // unique identifier
title: 'string' // campaign title
}
```

- Actions to implement:
- addCampaign → add a new campaign with unique id.
- updateCampaign → update an existing campaign title by id.
- deleteCampaign → delete a campaign by id.
- Reducer handles actions and updates state immutably.

**4. Redux Store**
- Configured with configureStore from Redux Toolkit (or optional traditional reducer/action setup).
- Provides the campaign slice globally if using Provider.

**Folder Structure**
```
src/
│
├── components/
│ ├── CampaignForm.jsx # Form to add campaigns
│ └── CampaignList.jsx # List to view, edit, and delete campaigns
│
├── redux/
│ ├── slice.js # Redux Toolkit slice: actions + reducer
│ ├── store.js # Redux store configuration
│ ├── actions.js # Optional: traditional Redux actions
│ └── reducer.js # Optional: traditional reducer
│
├── tests/
│ ├── App.test.js # Component tests (add, edit, delete)
│ └── Redux.test.js # Redux tests (add, update, delete campaigns)
│
├── App.js # Optional wrapper / main component
└── index.js # ReactDOM render
```

**Test Case Descriptions**

**Redux Tests (Redux.test.js)**

1. Initial state → should be empty.
2. Add campaign → verify new campaign is added with a unique id.
3. Update campaign → verify title updates correctly.
4. Delete campaign → verify campaign is removed by id.
5. Unknown actions → reducer should return previous state unchanged.

**Component Tests (App.test.js)**

1. Render CampaignForm and CampaignList → inputs and buttons should be in the document.
2. Add campaign via form → input value should appear in the list; input should reset.
3. Update campaign inline → editing the input should update the campaign title in the list.
4. Delete campaign → clicking delete removes campaign from the list.

**Steps to Run the Application**

Navigate to the project directory:
```
cd reactapp
```

Install dependencies:
```
npm install
```

Start the app:
```
npm start
```

Run test cases:
```
npm test
```

**Notes / Requirements**

- Campaign title input placeholder: "Enter campaign title" (required for tests).
- Add button text: "Add Campaign" (required for tests).
- Component state must update dynamically so changes are reflected in the DOM.
- Redux slice should handle actions immutably.
- Components and Redux logic are tested separately:
- Components → App.test.js (may use local state or mocked store)
- Redux → Redux.test.js (directly test reducer + actions)

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── CampaignForm.jsx
│   │   └── CampaignList.jsx
│   ├── redux/
│   │   ├── slice.js
│   │   └── store.js
│   ├── App.js
│   └── index.js
Code Implementation
src/redux/slice.js
jsximport { createSlice } from '@reduxjs/toolkit';
import { v4 as uuidv4 } from 'uuid';

const campaignSlice = createSlice({
  name: 'campaigns',
  initialState: {
    campaigns: []
  },
  reducers: {
    addCampaign: (state, action) => {
      state.campaigns.push({
        id: uuidv4(),
        title: action.payload
      });
    },
    updateCampaign: (state, action) => {
      const { id, title } = action.payload;
      const campaign = state.campaigns.find(c => c.id === id);
      if (campaign) {
        campaign.title = title;
      }
    },
    deleteCampaign: (state, action) => {
      state.campaigns = state.campaigns.filter(c => c.id !== action.payload);
    }
  }
});

export const { addCampaign, updateCampaign, deleteCampaign } = campaignSlice.actions;
export default campaignSlice.reducer;
src/redux/store.js
jsximport { configureStore } from '@reduxjs/toolkit';
import campaignReducer from './slice';

const store = configureStore({
  reducer: {
    campaigns: campaignReducer
  }
});

export default store;
src/components/CampaignForm.jsx
jsximport React, { useState } from 'react';
import { useDispatch } from 'react-redux';
import { addCampaign } from '../redux/slice';

const CampaignForm = () => {
  const [title, setTitle] = useState('');
  const dispatch = useDispatch();

  const handleSubmit = (e) => {
    e.preventDefault();
    if (title.trim()) {
      dispatch(addCampaign(title));
      setTitle('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Enter campaign title"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <button type="submit">Add Campaign</button>
    </form>
  );
};

export default CampaignForm;
src/components/CampaignList.jsx
jsximport React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { updateCampaign, deleteCampaign } from '../redux/slice';

const CampaignList = () => {
  const campaigns = useSelector(state => state.campaigns.campaigns);
  const dispatch = useDispatch();
  const [editingId, setEditingId] = useState(null);
  const [editTitle, setEditTitle] = useState('');

  const handleEdit = (campaign) => {
    setEditingId(campaign.id);
    setEditTitle(campaign.title);
  };

  const handleUpdate = (id) => {
    if (editTitle.trim()) {
      dispatch(updateCampaign({ id, title: editTitle }));
      setEditingId(null);
      setEditTitle('');
    }
  };

  const handleDelete = (id) => {
    dispatch(deleteCampaign(id));
  };

  return (
    <div>
      <h2>Campaign List</h2>
      <ul>
        {campaigns.map(campaign => (
          <li key={campaign.id}>
            {editingId === campaign.id ? (
              <>
                <input
                  type="text"
                  value={editTitle}
                  onChange={(e) => setEditTitle(e.target.value)}
                />
                <button onClick={() => handleUpdate(campaign.id)}>Save</button>
                <button onClick={() => setEditingId(null)}>Cancel</button>
              </>
            ) : (
              <>
                <span>{campaign.title}</span>
                <button onClick={() => handleEdit(campaign)}>Edit</button>
                <button onClick={() => handleDelete(campaign.id)}>Delete</button>
              </>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default CampaignList;
src/App.js
jsximport React from 'react';
import { Provider } from 'react-redux';
import store from './redux/store';
import CampaignForm from './components/CampaignForm';
import CampaignList from './components/CampaignList';

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <h1>Campaign Manager</h1>
        <CampaignForm />
        <CampaignList />
      </div>
    </Provider>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install

# Start the application
npm start

# Run tests
npm test

# Click on PORT: 8081 to view output
```

---

## PHASE-4: API INTEGRATION

### Question No: 1 - User List Component with Material-UI Integration and API Fetching

**Project Type Question**

**User List Component with Material-UI Integration and API Fetching**

**1.Create the UserLIst Component**

Inside the `src/components` folder, create a new file named `UserList.jsx`.

- Imports:
Imported React, useState, and useEffect for React functionality.
Imported Material-UI components: Avatar, List, ListItem, ListItemAvatar, ListItemText, and Typography.
Imported the getAllUsers function from the user service.

- State Initialization:
Declared a state variable userList using the useState hook to store the fetched user data.

- Data Fetching with useEffect:
Used the useEffect hook to fetch user data from the API when the component mounts.
Created a fetchData function that calls getAllUsers, updates userList with the response data, and handles errors.

**2.Create the UserApi file**

- Inside the `src/services` folder, create a new file named `UserApi.js`.
- Imported the Axios library to make HTTP requests.
- Defined the base URI for the API as "http://localhost:3005".
- Created and exported an asynchronous function getAllUsers that makes a GET request to retrieve user data from the specified API endpoint.

**3.Navigate to the React App Folder**

Open your terminal and change your working directory to the React app folder named `reactapp`.

**4.Install Dependencies**

- Run the following command to install the necessary dependencies: `npm install`, `npm install canvas`
- Run the following command to install json-server dependency: `npm install json-server`

**5.Navigate to the Shared Folder**

Open your terminal and change your working directory to the Shared folder present inside "reactapp/src` and use the command to run json-server: "npx json-server --watch db.json -p 3005" .

**6.Start the Development Server**

After the installation is complete, start the React development server with the following command:`npm start`

View the Output by clicking on above link `port: 8081`.

**Note:** Don't change anything inside `__mocks__` and "shared" folder.

**Output:**

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   └── UserList.jsx
│   ├── services/
│   │   └── UserApi.js
│   ├── shared/
│   │   └── db.json
│   ├── App.js
│   └── index.js
Code Implementation
src/services/UserApi.js
jsximport axios from 'axios';

const URI = 'http://localhost:3005';

export const getAllUsers = async () => {
  return await axios.get(`${URI}/users`);
};
src/components/UserList.jsx
jsximport React, { useState, useEffect } from 'react';
import { Avatar, List, ListItem, ListItemAvatar, ListItemText, Typography } from '@mui/material';
import { getAllUsers } from '../services/UserApi';

const UserList = () => {
  const [userList, setUserList] = useState([]);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await getAllUsers();
        setUserList(response.data);
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    };

    fetchData();
  }, []);

  return (
    <div>
      <Typography variant="h4">User List</Typography>
      <List>
        {userList.map(user => (
          <ListItem key={user.id}>
            <ListItemAvatar>
              <Avatar>{user.name.charAt(0)}</Avatar>
            </ListItemAvatar>
            <ListItemText
              primary={user.name}
              secondary={user.email}
            />
          </ListItem>
        ))}
      </List>
    </div>
  );
};

export default UserList;
src/App.js
jsximport React from 'react';
import UserList from './components/UserList';
import './App.css';

function App() {
  return (
    <div className="App">
      <UserList />
    </div>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install
npm install canvas

# Navigate to shared folder and start json-server
cd src/shared
npx json-server --watch db.json -p 3005

# In another terminal, start the React app
cd reactapp
npm start

# Click on PORT: 8081 to view output
```

---

### Question No: 1 - Blog Post Viewer

**Project Type Question**

**Scenario / Problem Statement**

You have been assigned to build a Blog Post Viewer using React.js. The application should fetch posts from a remote API and display them dynamically.

The application will:
- Make HTTP GET requests to a public API (https://jsonplaceholder.typicode.com/posts)
- Render the first five posts from the API
- Handle loading and error states
- Write Jest test cases to verify different states of the component

**Tech Stack:**
- Frontend: React.js (running on port 8081)

**Features to Implement:**

**Fetch Post**
- Use fetch() to retrieve posts when the component mounts.
- Show only the first 5 posts using .slice(0, 5).

**Loading State**
- While the request is in progress, display: Loading posts...

**Error State**
- If the request fails or returns a non-200 response, display the error message: Error: <message>

**Post List**
- Once the request succeeds, display each post title inside an unordered list.

**Test Cases (Jest + React Testing Library)**
- Write unit tests to cover:
- Initial loading state
- Success state with 5 posts rendered
- Error message when fetch fails
- Error message when API returns non-200 response
- Ensures only 5 posts are rendered even if API returns more

**Folder Structure:**

**Component Overview**

**App.js**
- Main entry point that renders the PostList component.

**PostList.js**
- Responsible for:
- Fetching data from API
- Showing loader, error, and list view
- Managing state using useState and useEffect

**Output**

**Steps to run the Application :**

Open the terminal and follow the steps below:

**Step 1:** Use "cd reactapp" command to navigate to the "reactapp" directory.

**Step 2:** Use "npm i" command to install the node_modules.

**Step 3:** Use "npm start" command to start the application.

**Note :**
- Click "PORT: 8081" to view the output.
- Click "Run Test Case" button to run the testcases.
- If any error persists while running the app, delete the node_modules and reinstall them.

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   └── PostList.js
│   ├── App.js
│   └── index.js
Code Implementation
src/components/PostList.js
jsximport React, { useEffect, useState } from 'react';

function PostList() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await fetch('https://jsonplaceholder.typicode.com/posts');
        if (!res.ok) {
          throw new Error(`Status: ${res.status}`);
        }

        const data = await res.json();
        setPosts(data.slice(0, 5));
      } catch (err) {
        if (err.message.includes('Status')) {
          setError(err.message);
        } else {
          setError('Error fetching posts');
        }
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  if (loading) {
    return <p>Loading posts...</p>;
  }

  if (error) {
    return <p>{error}</p>;
  }

  return (
    <div>
      <h1>Posts</h1>
      <ul data-testid="post-list">
        {posts.map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default PostList;
src/App.js
jsximport React from 'react';
import PostList from './components/PostList';

function App() {
  return (
    <div className="App">
      <PostList />
    </div>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm i

# Start the application
npm start

# Click on PORT: 8081 to view output
# Click "Run Test Case" to run tests
```

---

### Question No: 1 - User Profile with Material-UI Integration and API Fetching

**Project Type Question**

**User Profile with Material-UI Integration and API Fetching**

**1.Create the UserForm Component**

Inside the `src/components` folder, create a new file named `UserForm.jsx`.

- Import React and useState:
In your React project, start by importing the React and useState from the 'react' library. This is necessary for creating functional components and managing state.

- Import Necessary Styles and Dependencies:
Import any necessary styles or CSS files for styling your form. In this example, a CSS file named 'Form.css' is imported.

- Import External Functions and Hooks:
Import any external functions or hooks required for the component. In this case, the saveUser function from the 'UserApi' service and the useNavigate hook from 'react-router-dom' are imported.

- Initialise State using useState:
Use the useState hook to initialise the component state. In this example, the state formData is created to manage user input for name, email, and phone.

- Create Event Handlers:
Define event handlers for handling changes in the input fields and form submission. The handleChangeEvent function updates the state when the user types in any of the input fields, and handleSubmit prevents the default form submission behavior and calls the saveUser function.

- Implement Form Markup:
Create the JSX markup for the form. Use the label and input elements for each form field (name, email, and phone). Attach the event handlers to the input fields to capture user input.

- Use JSX to Display the Form:
Render the form within a container div. This can be done using the form element, and the input fields are enclosed within div elements. The submit button triggers the handleSubmit function.

- Invoke Hooks for Navigation:
Use the useNavigate hook from 'react-router-dom' to manage navigation within the application. This hook allows you to programmatically navigate to different pages.

- Handle Form Submission:
When the form is submitted, the handleSubmit function is called. It prevents the default form submission behavior, calls the saveUser function with the form data, and navigates to the user profile page using the navigate function.

- Export the Component:
Export the UserForm component so that it can be used in other parts of your application.

**2.Create the UserProfile Component**

Inside the `src/components` folder, create a new file named `UserProfile.jsx`.

- Import React and Necessary Dependencies:
Begin by importing React, useEffect, and useState from the 'react' library. Additionally, import any other dependencies required for the component, such as functions from the 'UserApi' service, MUI components, and assets like images.

- Initialise State using useState:
Utilise the useState hook to initialize the state for the component. In this case, user and editMode states are created to manage user data and determine whether the component is in edit mode.

- Use useEffect for Fetching Data:
Implement the useEffect hook to fetch user data when the component mounts. The getUserById function is used to retrieve user information based on the id parameter from the URL.

- Handle Change Event:
Create the handleChangeEvent function to update the state when there are changes in the input fields. This function is used for handling changes in the user's name, email, and phone number.

- Implement Edit and Save Functions:
Define handleEditClick and handleSaveClick functions to toggle the edit mode and save the changes, respectively. When in edit mode, input fields are rendered, allowing the user to modify their information.

- Implement Delete Function:
Create the handleDeleteClick function to delete the user account. It calls the deleteUser function from the 'UserApi' service and updates the state accordingly.

- Render JSX Markup:
Use JSX to render the user profile information within a MUI Card component. Display user details such as name, email, and phone number. Use conditional rendering to show input fields when in edit mode.

- Handle Conditional Rendering for User Not Found:
Check if the user object is empty. If so, display a message and a button to navigate back. This handles the scenario where the user is not found.

- Export the Component:
Finally, export the UserProfile component so that it can be used in other parts of your React application.

**3.Create the UserApi file**

Inside the `src/services` folder, create a new file named `UserApi.js`.

- Import Axios:
Begin by importing the Axios library, which facilitates making HTTP requests.

- Define API Base URI:
Set the base URI for your API, assuming it to be 'http://localhost:3005'.

- Create saveUser Function:
Define the saveUser function, responsible for saving a new user to the API. It retrieves the total number of users, creates a new user object with an incremented ID, and sends a POST request to add the user to the API.

- Create getUserById Function:
Implement the getUserById function to fetch a user's data based on their ID. It sends a GET request to the API endpoint for the specific user.

- Create updateUser Function:
Implement the updateUser function to update a user's information. This function sends a PUT request to the API with the user's ID and updated data.

- Create deleteUser Function:
Define the deleteUser function to delete a user based on their ID. This function sends a DELETE request to the API with the user's ID.

- Handle Asynchronous Operations:
Ensure each function is marked as async to handle asynchronous operations, and use await when making requests to the API to wait for responses.

- Error Handling:
Implement error handling within each function using try-catch blocks to log errors to the console in case of failures.

- Export Functions:
Export each function so that they can be imported and used in other parts of your application.

**4.Inside App.js file**

- Define Routes:
Utilize the Route component from 'react-router-dom' to specify routes within the Routes component. Assign paths and corresponding components for rendering when paths are matched.

The root path ('/') renders the UserForm component.

The '/user-profile/:id' path renders the UserProfile component, dynamically adjusting based on the user's ID.

**5.Navigate to the React App Folder**

Open your terminal and change your working directory to the React app folder named `reactapp`.

**6.Install Dependencies**

- Run the following command to install the necessary dependencies: `npm install`, `npm install canvas`
- Run the following command to install json-server dependency: `npm install json-server`

**7.Navigate to the Shared Folder**

Open your terminal and change your working directory to the Shared folder present inside "reactapp/src` and use the command to run json-server: "npx json-server --watch db.json -p 3005" .

**8.Start the Development Server**

After the installation is complete, start the React development server with the following command:`npm start`

View the Output by clicking on above link `port: 8081`.

**Note:** Don't change anything inside `__mocks__` and "shared" folder.

#### Answer:

**File Structure**
```
reactapp/
├── src/
│   ├── components/
│   │   ├── UserForm.jsx
│   │   └── UserProfile.jsx
│   ├── services/
│   │   └── UserApi.js
│   ├── shared/
│   │   └── db.json
│   ├── App.js
│   └── index.js
Code Implementation
src/services/UserApi.js
jsximport axios from 'axios';

const URI = 'http://localhost:3005';

export const saveUser = async (userData) => {
  try {
    const response = await axios.get(`${URI}/users`);
    const users = response.data;
    const newId = users.length > 0 ? Math.max(...users.map(u => u.id)) + 1 : 1;
    
    const newUser = {
      ...userData,
      id: newId
    };
    
    return await axios.post(`${URI}/users`, newUser);
  } catch (error) {
    console.error('Error saving user:', error);
    throw error;
  }
};

export const getUserById = async (id) => {
  try {
    return await axios.get(`${URI}/users/${id}`);
  } catch (error) {
    console.error('Error fetching user:', error);
    throw error;
  }
};

export const updateUser = async (id, userData) => {
  try {
    return await axios.put(`${URI}/users/${id}`, userData);
  } catch (error) {
    console.error('Error updating user:', error);
    throw error;
  }
};

export const deleteUser = async (id) => {
  try {
    return await axios.delete(`${URI}/users/${id}`);
  } catch (error) {
    console.error('Error deleting user:', error);
    throw error;
  }
};
src/components/UserForm.jsx
jsximport React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { saveUser } from '../services/UserApi';
import './Form.css';

const UserForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: ''
  });
  
  const navigate = useNavigate();

  const handleChangeEvent = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await saveUser(formData);
      navigate(`/user-profile/${response.data.id}`);
    } catch (error) {
      console.error('Error submitting form:', error);
    }
  };

  return (
    <div className="form-container">
      <h2>User Registration</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="name">Name</label>
          <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChangeEvent}
            required
          />
        </div>
        <div>
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={handleChangeEvent}
            required
          />
        </div>
        <div>
          <label htmlFor="phone">Phone</label>
          <input
            type="tel"
            id="phone"
            name="phone"
            value={formData.phone}
            onChange={handleChangeEvent}
            required
          />
        </div>
        <button type="submit">Submit</button>
      </form>
    </div>
  );
};

export default UserForm;
src/components/UserProfile.jsx
jsximport React, { useState, useEffect } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import { Card, CardContent, Typography, Button } from '@mui/material';
import { getUserById, updateUser, deleteUser } from '../services/UserApi';

const UserProfile = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const [user, setUser] = useState({});
  const [editMode, setEditMode] = useState(false);

  useEffect(() => {
    const fetchUser = async () => {
      try {
        const response = await getUserById(id);
        setUser(response.data);
      } catch (error) {
        console.error('Error fetching user:', error);
      }
    };

    fetchUser();
  }, [id]);

  const handleChangeEvent = (e) => {
    setUser({
      ...user,
      [e.target.name]: e.target.value
    });
  };

  const handleEditClick = () => {
    setEditMode(true);
  };

  const handleSaveClick = async () => {
    try {
      await updateUser(id, user);
      setEditMode(false);
    } catch (error) {
      console.error('Error updating user:', error);
    }
  };

  const handleDeleteClick = async () => {
    try {
      await deleteUser(id);
      navigate('/');
    } catch (error) {
      console.error('Error deleting user:', error);
    }
  };

  if (!user.id) {
    return (
      <div>
        <p>User not found</p>
        <Button onClick={() => navigate('/')}>Back</Button>
      </div>
    );
  }

  return (
    <Card>
      <CardContent>
        <Typography variant="h4">User Profile</Typography>
        {editMode ? (
          <div>
            <input
              type="text"
              name="name"
              value={user.name}
              onChange={handleChangeEvent}
            />
            <input
              type="email"
              name="email"
              value={user.email}
              onChange={handleChangeEvent}
            />
            <input
              type="tel"
              name="phone"
              value={user.phone}
              onChange={handleChangeEvent}
            />
            <Button onClick={handleSaveClick}>Save</Button>
          </div>
        ) : (
          <div>
            <Typography>Name: {user.name}</Typography>
            <Typography>Email: {user.email}</Typography>
            <Typography>Phone: {user.phone}</Typography>
            <Button onClick={handleEditClick}>Edit</Button>
            <Button onClick={handleDeleteClick}>Delete</Button>
          </div>
        )}
      </CardContent>
    </Card>
  );
};

export default UserProfile;
src/App.js
jsximport React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import UserForm from './components/UserForm';
import UserProfile from './components/UserProfile';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <Routes>
          <Route path="/" element={<UserForm />} />
          <Route path="/user-profile/:id" element={<UserProfile />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
Steps to Run
bash# Navigate to reactapp directory
cd reactapp

# Install dependencies
npm install
npm install canvas

# Navigate to shared folder and start json-server
cd src/shared
npx json-server --watch db.json -p 3005

# In another terminal, start the React app
cd reactapp
npm start

# Click on PORT: 8081 to view output

END OF DOCUMENTATION
