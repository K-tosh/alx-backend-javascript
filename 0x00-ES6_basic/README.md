# Curriculum

## Short Specializations


### Concepts
For this project, we expect you to look at these concepts:
- JavaScript programming
- Software Linter

### Resources
Read or watch:
- ECMAScript 6 - ECMAScript 2015
- Statements and declarations
- Arrow functions
- Default parameters
- Rest parameter
- Javascript ES6 — Iterables and Iterators

### Learning Objectives
At the end of this project, you are expected to be able to explain to anyone, without the help of Google:
- What ES6 is
- New features introduced in ES6
- The difference between a constant and a variable
- Block-scoped variables
- Arrow functions and function parameters default to them
- Rest and spread function parameters
- String templating in ES6
- Object creation and their properties in ES6
- Iterators and for-of loops

### Requirements
General:
- All your files will be executed on Ubuntu 18.04 LTS using NodeJS 12.11.x
- Allowed editors: vi, vim, emacs, Visual Studio Code
- All your files should end with a new line
- A README.md file, at the root of the folder of the project, is mandatory
- Your code should use the js extension
- Your code will be tested using the Jest Testing Framework
- Your code will be analyzed using the linter ESLint along with specific rules that we’ll provide
- All of your functions must be exported

### Setup
Install NodeJS 12.11.x (in your home directory):
```
curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs -y
$ nodejs -v
v12.11.1
$ npm -v
6.11.3
```
Install Jest, Babel, and ESLint in your project directory, install Jest, Babel and ESList by using the supplied package.json and run npm install.

### Configuration files
Add the following files to your project directory:

package.json
```
{
    "scripts": {
        "lint": "./node_modules/.bin/eslint",
        "check-lint": "lint [0-9]*.js",
        "dev": "npx babel-node",
        "test": "jest",
        "full-test": "./node_modules/.bin/eslint [0-9]*.js && jest"
    },
    "devDependencies": {
        "@babel/core": "^7.6.0",
        "@babel/node": "^7.8.0",
        "@babel/preset-env": "^7.6.0",
        "eslint": "^6.4.0",
        "eslint-config-airbnb-base": "^14.0.0",
        "eslint-plugin-import": "^2.18.2",
        "eslint-plugin-jest": "^22.17.0",
        "jest": "^24.9.0"
    }
}
```

babel.config.js
```
module.exports = {
    presets: [
        [
            '@babel/preset-env',
            {
                targets: {
                    node: 'current',
                },
            },
        ],
    ],
};
```

.eslintrc.js
```
module.exports = {
    env: {
        browser: false,
        es6: true,
        jest: true,
    },
    extends: [
        'airbnb-base',
        'plugin:jest/all',
    ],
    globals: {
        Atomics: 'readonly',
        SharedArrayBuffer: 'readonly',
    },
    parserOptions: {
        ecmaVersion: 2018,
        sourceType: 'module',
    },
    plugins: ['jest'],
    rules: {
        'no-console': 'off',
        'no-shadow': 'off',
        'no-restricted-syntax': [
            'error',
            'LabeledStatement',
            'WithStatement',
        ],
    },
    overrides:[
        {
            files: ['*.js'],
            excludedFiles: 'babel.config.js',
        }
    ]
};
```

Finally…
Don’t forget to run npm install from the terminal of your project folder to install all necessary project dependencies.

### Tasks
#### 0. Const or let?
mandatory
Modify the function taskFirst to instantiate variables using const and the function taskNext to instantiate variables using let.

```javascript
export function taskFirst() {
    const task = 'I prefer const when I can.';
    return task;
}

export function getLast() {
    return ' is okay';
}

export function taskNext() {
    let combination = 'But sometimes let';
    combination += getLast();

    return combination;
}
```

#### 1. Block Scope
mandatory
Given what you’ve read about var and hoisting, modify the variables inside the function taskBlock so that the variables aren’t overwritten inside the conditional block.

```javascript
export default function taskBlock(trueOrFalse) {
    let task = false;
    let task2 = true;

    if (trueOrFalse) {
        task = true;
        task2 = false;
    }

    return [task, task2];
}
```

#### 2. Arrow functions
mandatory
Rewrite the following standard function to use ES6’s arrow syntax of the function add (it will be an anonymous function after).

```javascript
export default function getNeighborhoodsList() {
    this.sanFranciscoNeighborhoods = ['SOMA', 'Union Square'];

    const self = this;
    this.addNeighborhood = function add(newNeighborhood) {
        self.sanFranciscoNeighborhoods.push(newNeighborhood);
        return self.sanFranciscoNeighborhoods;
    };
}
```

#### 3. Parameter defaults
mandatory
Condense the internals of the following function to 1 line - without changing the name of each function/variable.

```javascript
export default function getSumOfHoods(initialNumber, expansion1989, expansion2019) {
    if (expansion1989 === undefined) {
        expansion1989 = 89;
    }

    if (expansion2019 === undefined) {
        expansion2019 = 19;
    }
    return initialNumber + expansion1989 + expansion2019;
}
```

#### 4. Rest parameter syntax for functions
mandatory
Modify the following function to return the number of arguments passed to it using the rest parameter syntax.

```javascript
export default function returnHowManyArguments(...args) {
    return args.length;
}
```

#### 5. The wonders of spread syntax
mandatory
Using spread syntax, concatenate 2 arrays and each character of a string by modifying the function below. Your function body should be one line long.

```javascript
export default function concatArrays(array1, array2, string) {
    return [...array1, ...array2, ...string];
}
```

#### 6. Take advantage of template literals
mandatory
Rewrite the return statement to use a template literal so you can substitute the variables you’ve defined.

```javascript
export default function getSanFranciscoDescription() {
    const year = 2017;
    const budget = {
        income: '$119,868',
        gdp: '$154.2 billion',
        capita: '$178,479',
    };

    return `As of ${year}, it was the seventh-highest income county in the United States, with a per capita personal income of ${budget.income}. As of 2015, San Francisco proper had a GDP of ${budget.gdp}, and a GDP per capita of ${budget.capita}.`;
}
```

#### 7. Object property value shorthand syntax
mandatory
Modify the following function’s budget object to simply use the keyname instead.

```javascript
export default function getBudgetObject(income, gdp, capita) {
    const budget = {
        income,
        gdp,
        capita,
    };

    return budget;
}
```

#### 8. No need to create empty objects before adding in properties
mandatory
Rewrite the getBudgetForCurrentYear function to use ES6 computed property names on the budget object.

```javascript
function getCurrentYear() {
    const date = new Date();
    return date.getFullYear();
}

export default function getBudgetForCurrentYear(income, gdp, capita) {
    const budget = {
        [`income-${getCurrentYear()}`]: income,
        [`gdp-${getCurrentYear()}`]: gdp,
        [`capita-${getCurrentYear()}`]: capita,
    };

    return budget;
}
```

#### 9. ES6 method properties
mandatory
Rewrite getFullBudgetObject to use ES6 method properties in the fullBudget object.

```javascript
import getBudgetObject from './7-getBudgetObject.js';

export default function getFullBudgetObject(income, gdp, capita) {
    const budget = getBudgetObject(income, gdp, capita);
    const fullBudget = {
        ...budget,
        getIncomeInDollars(income) {
            return `$${income}`;
        },
        getIncomeInEuros(income) {
            return `${income} euros`;
        },
    };

    return fullBudget;
}
```

#### 10. For...of Loops
mandatory
Rewrite the function appendToEachArrayValue to use ES6’s for...of operator. And don’t forget that var is not ES6-friendly.

```javascript
export default function appendToEachArrayValue(array, appendString) {
    for (const value of array) {
        value = appendString + value;
    }

    return array;
}
```

#### 11. Iterator
mandatory
Write a function named createEmployeesObject that will receive two arguments: departmentName (String) and employees (Array of Strings).

```javascript
export default function createEmployeesObject(departmentName, employees) {
    return {
        [departmentName]: employees,
    };
}
```

#### 12. Let's create a report object
mandatory
Write a function named createReportObject whose parameter, employeesList, is the return value of the previous function createEmployeesObject.

```javascript
export default function createReportObject(employeesList) {
    return {
        allEmployees: employeesList,
        getNumberOfDepartments() {
            return Object.keys(employeesList).length;
        },
    };
}
```

