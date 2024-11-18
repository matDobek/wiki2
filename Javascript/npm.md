
### tl;dr

```
npm install -g express

# package.json
npm install 
npm install express
npm install express@latest
npm install express@9.99.999
npm install express --save-prod
npm install jest --save-dev

npm uninstall express
npm update express

npm start # runs the start script defined in package.json
npm run <script> # runs any custom script
npm run test
npm run build
```

### What `-D` stand for in `npm install -D vitest` ?

```
`--save-dev` or `-D`
```

### Difference between `Vite.js`, `webpack`, `create react app ( CRA ) ?

Those tools are used to simplify building an application

For local development:
* hot reloading
* fast building / rebuilding

For production:
* bundle code together, minifying and optimizing

CRA uses `webpack` with sane defaults.
`vite` is newer

```
# Create app
npm create vite my-app

# Select react as framework

# Install dependencies
cd my-app
npm install three @react-three/fiber
npm install @react-three/drei


# Start development server
npm run dev
```

### What is the difference between `npm` and `npx`?
`npm` is used to install global or local packages
`npx` is used to execute package without installing it locally

You may want to install `eslint` - `npm`
But not `create-react-app` - as this is only used once, for bootstraping