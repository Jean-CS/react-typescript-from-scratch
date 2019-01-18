# React from scratch with Webpack and TypeScript

1. Install React `npm i react react-dom`
2. Install Development dependencies:

   ```javascript
    npm i -D awesome-typescript-loader @types/react @types/react-dom html-webpack-plugin @types/html-webpack-plugin typescript webpack webpack-cli @types/webpack webpack-dev-server ts-node
   ```

3. Set up project structure like this:

    ```terminal
    - react-from-scratch
        - node_modules
        - src
            - components
                - app.tsx
        - index.html
        - index.tsx
        - package.json
    ```

4. Create two `webpack` config giles. One for _development_ `webpack.dev.ts` and the other for _production_ `webpack.prod.ts`

    > We can make only one configuration file for both environments, but in this project let's try keeping things separate

- Contents for `webpack.dev.ts`

```javascript
import * as webpack from "webpack";
import * as HtmlWebPackPlugin from "html-webpack-plugin";

const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html"
});

const config: webpack.Configuration = {
  mode: "development",
  entry: "./src/index.tsx",
  resolve: {
    // Add '.ts' and '.tsx' as resolvable extensions.
    extensions: [".ts", ".tsx", ".js", ".json"]
  },

  module: {
    rules: [
      // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
      { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
    ]
  },
  plugins: [htmlPlugin]
};

export default config;
```

- Contents for `webpack.prod.ts`

```javascript
import * as path from "path";
import * as webpack from "webpack";
import * as HtmlWebPackPlugin from "html-webpack-plugin";

const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

const config: webpack.Configuration = {
  mode: "production",
  entry: "./src/index.tsx",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js"
  },
  resolve: {
    // Add '.ts' and '.tsx' as resolvable extensions.
    extensions: [".ts", ".tsx", ".js", ".json"]
  },

  module: {
    rules: [
      // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
      { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
    ]
  },
  plugins: [htmlPlugin]
};

export default config;
```

The difference between the development and the production webpack file is that we will transpile the code and make a new folder with files in them for the production build when running webpack.prod.ts and we don’t do that for the development configuration.

In the webpack.prod.ts file, the html-webpack-plugin does a little more than just fire up the index.html template from our src folder, it also creates an index.html in our dist folder.

5. Add the following lines in `package.json`

```javascript
"scripts": {
    "dev": "webpack-dev-server --open --config webpack.dev.ts",
    "build": "webpack --mode production --config webpack.prod.ts"
},
```

6. Setup compiler options for the typescript configurations inside `tsconfig.json`

```javascript
{
    "compilerOptions": {
        "outDir": "./dist/",
        "noImplicitAny": true,
        "target": "es5",
        "lib": ["es5", "es6", "dom"],
        "jsx": "react",
        "allowJs": true,
        "moduleResolution": "node"
    }
}
```

More information on TypeScript configurations see [here](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

7. To run on dev: `npm run dev`
8. To run on prod: `npm run build`