# Getting Started with SimpleKit

SimpleKit is a small TypeScript user interface toolkit. It can be installed as an npm package, linked from a cloned repository or added as a git submodule. This guide explains all three approaches and also describes how to build the source code.

## 1. Installing the npm package

The easiest way to experiment with SimpleKit is to install the published package. Create a node project and run:

```bash
npm install simplekit
```

You can then import modules from `simplekit` in your TypeScript code, for example:

```ts
import { startSimpleKit } from "simplekit/imperative-mode";
```

This approach works well if you already have a TypeScript build system in place. Updating to new releases is as simple as running `npm update`.

## 2. Using `npm link`

If you wish to edit the toolkit while developing your project you can work with a cloned repository and `npm link`.

1. Clone the repository and install dependencies:
   ```bash
   git clone https://github.com/nonsequitoria/simplekit.git
   cd simplekit
   npm install
   ```
2. From the repository root, run `npm link` to create a globally linked package.
3. In your project directory run `npm link simplekit` to reference the local clone.

Any changes to the library will be available immediately once TypeScript recompiles. Visual Studio Code may occasionally lose type information for linked packages. Restarting the TypeScript server ("TypeScript: Restart TS Server" from the command palette) resolves this.

## 3. Adding as a git submodule

Projects can also include the entire SimpleKit repository as a submodule. Inside your project run:

```bash
git submodule add https://github.com/nonsequitoria/simplekit.git simplekit
```

After cloning the main project on another machine remember to initialise and update submodules:

```bash
git submodule update --init --recursive
```

Imports can then use relative paths to the `simplekit` directory or you can configure path aliases. For a Vite based project add an alias in `vite.config.ts`:

```ts
import { defineConfig } from "vite";
import tsconfigPaths from "vite-tsconfig-paths";

export default defineConfig({
  plugins: [tsconfigPaths()],
  resolve: {
    alias: {
      simplekit: path.resolve(__dirname, "./simplekit/src"),
    },
  },
});
```

And in `tsconfig.json` add:

```json
{
  "compilerOptions": {
    "paths": {
      "simplekit/*": ["simplekit/src/*"]
    }
  }
}
```

## Building SimpleKit

The repository includes a small build script powered by [tsup](https://github.com/egoist/tsup). The following commands are available:

- `npm run build` – compiles the sources under `src/` to the `dist` directory.
- `npm run start` – watches the sources and rebuilds on change.
- `npm test` – runs the Jest test suite found under `__tests__/`.

You rarely need to build when using `npm link` or the submodule approach, but it is useful when publishing the package or running tests.
