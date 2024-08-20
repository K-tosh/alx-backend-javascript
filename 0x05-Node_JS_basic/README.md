# 0x05. NodeJS Basics

## Back-end
- JavaScript
- ES6
- NodeJS
- ExpressJS


## Resources
Read or watch:

- Node JS getting started
- Process API doc
- Child process
- Express getting started
- Mocha documentation
- Nodemon documentation

## Learning Objectives
At the end of this project, you are expected to be able to explain to anyone, without the help of Google:

- Run JavaScript using NodeJS
- Use NodeJS modules
- Use specific Node JS module to read files
- Use process to access command line arguments and the environment
- Create a small HTTP server using Node JS
- Create a small HTTP server using Express JS
- Create advanced routes with Express JS
- Use ES6 with Node JS with Babel-node
- Use Nodemon to develop faster

## Requirements
- Allowed editors: vi, vim, emacs, Visual Studio Code
- All your files will be interpreted/compiled on Ubuntu 18.04 LTS using node (version 12.x.x)
- All your files should end with a new line
- A README.md file, at the root of the folder of the project, is mandatory
- Your code should use the js extension
- Your code will be tested using Jest and the command npm run test
- Your code will be verified against lint using ESLint
- Your code needs to pass all the tests and lint. You can verify the entire project running npm run full-test
- All of your functions/classes must be exported by using this format: module.exports = myFunction;

## Provided files
- database.csv
- firstname,lastname,age,field
- Johann,Kerbrou,30,CS
- Guillaume,Salou,30,SWE
- Arielle,Salou,20,CS
- Jonathan,Benou,30,CS
- Emmanuel,Turlou,40,CS
- Guillaume,Plessous,35,CS
- Joseph,Crisou,34,SWE
- Paul,Schneider,60,SWE
- Tommy,Schoul,32,SWE
- Katie,Shirou,21,CS

- package.json
- babel.config.js
- .eslintrc.js

## Tasks
### 0. Executing basic javascript with Node JS
In the file 0-console.js, create a function named displayMessage that prints in STDOUT the string argument.

```javascript
const displayMessage = (message) => {
    console.log(message);
};

module.exports = displayMessage;
```

### 1. Using Process stdin
Create a program named 1-stdin.js that will be executed through command line:

It should display the message Welcome to Holberton School, what is your name? (followed by a new line)
The user should be able to input their name on a new line
The program should display Your name is: INPUT
When the user ends the program, it should display This important software is now closing (followed by a new line)

```javascript
process.stdin.setEncoding('utf8');

process.stdout.write('Welcome to Holberton School, what is your name?\n');

process.stdin.on('data', (data) => {
    const name = data.toString().trim();
    if (name) {
        process.stdout.write(`Your name is: ${name}\n`);
    }
});

process.stdin.on('end', () => {
    process.stdout.write('This important software is now closing\n');
});
```

### 2. Reading a file synchronously with Node JS
Using the database database.csv (provided in project description), create a function countStudents in the file 2-read_file.js

```javascript
const fs = require('fs');

const countStudents = (path) => {
    try {
        const data = fs.readFileSync(path, 'utf8');
        const lines = data.split('\n').filter((line) => line);
        const students = lines.map((line) => line.split(',')[0]);
        const csStudents = students.filter((student) => student.split(',')[3] === 'CS');
        const sweStudents = students.filter((student) => student.split(',')[3] === 'SWE');
        console.log(`Number of students: ${students.length}`);
        console.log(`Number of students in CS: ${csStudents.length}. List: ${csStudents.join(', ')}`);
        console.log(`Number of students in SWE: ${sweStudents.length}. List: ${sweStudents.join(', ')}`);
    } catch (error) {
        console.error('Cannot load the database');
    }
};

module.exports = countStudents;
```

### 3. Reading a file asynchronously with Node JS
Using the database database.csv (provided in project description), create a function countStudents in the file 3-read_file_async.js

```javascript
const fs = require('fs');

const countStudents = (path) => {
    return new Promise((resolve, reject) => {
        fs.readFile(path, 'utf8', (error, data) => {
            if (error) {
                reject(new Error('Cannot load the database'));
            } else {
                const lines = data.split('\n').filter((line) => line);
                const students = lines.map((line) => line.split(',')[0]);
                const csStudents = students.filter((student) => student.split(',')[3] === 'CS');
                const sweStudents = students.filter((student) => student.split(',')[3] === 'SWE');
                console.log(`Number of students: ${students.length}`);
                console.log(`Number of students in CS: ${csStudents.length}. List: ${csStudents.join(', ')}`);
                console.log(`Number of students in SWE: ${sweStudents.length}. List: ${sweStudents.join(', ')}`);
                resolve();
            }
        });
    });
};

module.exports = countStudents;
```

### 4. Create a small HTTP server using Node's HTTP module
In a file named 4-http.js, create a small HTTP server using the http module:

```javascript
const http = require('http');

const app = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello Holberton School!\n');
});

module.exports = app;
```

### 5. Create a more complex HTTP server using Node's HTTP module
In a file named 5-http.js, create a small HTTP server using the http module:

```javascript
const http = require('http');
const { countStudents } = require('./3-read_file_async');

const app = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    if (req.url === '/') {
        res.end('Hello Holberton School!\n');
    } else if (req.url === '/students') {
        countStudents('database.csv')
            .then(() => {
                res.end();
            })
            .catch((error) => {
                res.statusCode = 500;
                res.end(error.message);
            });
    } else {
        res.statusCode = 404;
        res.end('Not found\n');
    }
});

module.exports = app;
```

### 6. Create a small HTTP server using Express
Install Express and in a file named 6-http_express.js, create a small HTTP server using Express module:

```javascript
const express = require('express');

const app = express();
const port = 1245;

app.get('/', (req, res) => {
    res.send('Hello Holberton School!');
});

module.exports = app;
```

### 7. Create a more complex HTTP server using Express
In a file named 7-http_express.js, recreate the small HTTP server using Express:

```javascript
const express = require('express');
const { countStudents } = require('./3-read_file_async');

const app = express();
const port = 1245;

app.get('/', (req, res) => {
    res.send('Hello Holberton School!');
});

app.get('/students', (req, res) => {
    countStudents('database.csv')
        .then(() => {
            res.end();
        })
        .catch((error) => {
            res.status(500).send(error.message);
        });
});

module.exports = app;
```

### 8. Organize a complex HTTP server using Express
Obviously writing every part of a server within a single file is not sustainable. Let’s create a full server in a directory named full_server.

Since you have used ES6 and Babel in the past projects, let’s use babel-node to allow to use ES6 functions like import or export.

#### 8.1 Organize the structure of the server
Create 2 directories within:
- controllers
- routes

Create a file full_server/utils.js, in the file create a function named readDatabase that accepts a file path as argument:

```javascript
import fs from 'fs';

const readDatabase = (path) => {
    return new Promise((resolve, reject) => {
        fs.readFile(path, 'utf8', (error, data) => {
            if (error) {
                reject(new Error('Cannot load the database'));
            } else {
                const lines = data.split('\n').filter((line) => line);
                const students = lines.map((line) => line.split(',')[0]);
                const csStudents = students.filter((student) => student.split(',')[3] === 'CS');
                const sweStudents = students.filter((student) => student.split(',')[3] === 'SWE');
                resolve({
                    students,
                    csStudents,
                    sweStudents,
                });
            }
        });
    });
};

export default readDatabase;
```

#### 8.2 Write the App controller
Inside the file full_server/controllers/AppController.js:

```javascript
class AppController {
    static getHomepage(req, res) {
        res.status(200).send('Hello Holberton School!');
    }
}

export default AppController;
```

#### 8.3 Write the Students controller
Inside the file full_server/controllers/StudentsController.js, create a class named StudentsController. Add two static methods:

```javascript
import readDatabase from '../utils';

class StudentsController {
    static async getAllStudents(req, res) {
        try {
            const { students, csStudents, sweStudents } = await readDatabase('database.csv');
            res.status(200).send(`This is the list of our students\nNumber of students: ${students.length}\nNumber of students in CS: ${csStudents.length}. List: ${csStudents.join(', ')}\nNumber of students in SWE: ${sweStudents.length}. List: ${sweStudents.join(', ')}`);
        } catch (error) {
            res.status(500).send(error.message);
        }
    }

    static async getAllStudentsByMajor(req, res) {
        try {
            const { students, csStudents, sweStudents } = await readDatabase('database.csv');
            const { major } = req.params;
            if (major === 'CS') {
                res.status(200).send(`List: ${csStudents.join(', ')}`);
            } else if (major === 'SWE') {
                res.status(200).send(`List: ${sweStudents.join(', ')}`);
            } else {
                res.status(500).send('Major parameter must be CS or SWE');
            }
        } catch (error) {
            res.status(500).send(error.message);
        }
    }
}

export default StudentsController;
```

#### 8.4 Write the routes
Inside the file full_server/routes/index.js:

```javascript
import express from 'express';
import AppController from '../controllers/AppController';
import StudentsController from '../controllers/StudentsController';

const router = express.Router();

router.get('/', AppController.getHomepage);
router.get('/students', StudentsController.getAllStudents);
router.get('/students/:major', StudentsController.getAllStudentsByMajor);

export default router;
```

#### 8.5 Write the server reusing everything you created
Inside the file named full_server/server.js, create a small Express server:

```javascript
import express from 'express';
import router from './routes';

const app = express();
const port = 1245;

app.use('/', router);

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

#### 8.6 Update package.json (if you are running it from outside the folder full_server)
If you are starting node from outside of the folder full_server, you will have to update the command dev by: nodemon --exec babel-node --presets babel-preset-env ./full_server/server.js ./database.csv

Warning:

Don’t forget to export your express app at the end of server.js (export default app;)
The database filename is passed as argument of the server.js BUT, for testing purpose, you should retrieve this filename at the execution (when getAllStudents or getAllStudentsByMajor are called for example)

```json
{
    "name": "alx-backend-javascript",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "full-test": "npm run test && npm run lint",
        "lint": "eslint .",
        "dev": "nodemon --exec babel-node --presets babel-preset-env ./full_server/server.js ./database.csv"
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "@babel/cli": "^7.10.5",
        "@babel/core": "^7.10.5",
        "@babel/node": "^7.10.5",
        "@babel/preset-env": "^7.10.4",
        "eslint": "^7.5.0",
        "eslint-config-airbnb-base": "^14.2.0",
        "eslint-plugin-import": "^2.22.0",
        "nodemon": "^2.0.4"
    },
    "dependencies": {
        "express": "^4.17.1"
    }
}
```
