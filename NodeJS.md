# My notes on Node.JS and npm

## How to use npm
npm is the package manager for Node just like pip is for Python

### Setting up development environment
- A best practice while developing applications in Node is to `cd` into your project directory and use to the command `npm init` to generate a `package.json` for the project and fill it with the required details.
- If you call `npm  init` with any of these flags: `-f`, `--force`, `-y` and `--yes` it will use the default options for every field without prompting the user for input.

### User account
Every developer can create an account on the npm site and also login from their terminal to publish packages straight from your shell. Here are some commands to help with that:
- `npm adduser` to create a new npm account from the teminal and also save your login credentials
- `npm whoami` to display your npm user name
- `npm login` to login to your account from the shell. Once logged in from the shell a token can be seen on the npm website on your user account page.

### Installing modules
- `npm install <modulename>` or `npm install @<npm-username-of-the-author>/<package-name>`

### Listing dependencies
- `npm ls` command lists all the dependencies used by the application.

### Running tests
- `npm test` will run the test file defined in the scripts field that acts as an entry point for testing
```
"scripts": {  
         "test": "node test.js"  
},  
```

### Publishing
- To publish your package use `npm publish` command.

**Note:** Always update the package version before publishing otherwise it will throw an error.

### Versioning
npm follows semantic versioning and the npm version in the package.json file can be updated everytime we publish a new update to our package using `npm version` command

- For a patch update `npm version patch`. For e.g cahnges `v1.0.0 to v1.0.1` (for patches to bug fixes).
- For a minor update `npm version minor`. For e.g cahnges `v1.0.0 to v1.1.0` (for new additions to the API).
- For a major update `npm version major`. For e.g cahnges `v1.0.0 to v2.0.0` (for breaking API changes).

### Distribution
- Every published package on npm has a `dist-tag` which maps strings like `latest` to version numbers.
- By default the latest version gets installed.
- If one does not want to make the latest release the default version of the package then this can be done by changing the `dist-tag`
