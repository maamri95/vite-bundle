## Manual installation

```console
composer require pentatrion/vite-bundle
```

if you do not want to use the recipe or want to see in depth what is modified by it, create a directory structure for your js/css files:

```
├──assets
│ ├──app.js
│ ├──app.css
│...
├──public
├──composer.json
├──package.json
├──vite.config.js
```

add vite route to your dev Symfony app.

```yaml
# config/routes/dev/pentatrion_vite.yaml
_pentatrion_vite:
    prefix: /build
    resource: "@PentatrionViteBundle/Resources/config/routing.yaml"
```

create or complete your `package.json`

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite build"
    },
    "devDependencies": {
        "vite": "^2.6",
        "vite-plugin-symfony": "^0.1.2"
    }
}
```

create a `vite.config.js` file on your project root directory.
the symfonyPlugin and the `manifest: true` are required for the bundle to work. when you run the `npm run dev` the plugin remove the manifest.json file so ViteBundle know that he must return the served files.
when you run the `npm run build` the manifest.json is constructed and ViteBundle read his content to return the build files.

```js
// vite.config.js
import {defineConfig} from "vite";
import symfonyPlugin from "vite-plugin-symfony";

/* if you're using React */
// import reactRefresh from "@vitejs/plugin-react-refresh";

export default defineConfig({
    plugins: [
        /* reactRefresh(), // if you're using React */
        symfonyPlugin(),
    ],
    root: "./assets",

    /* your outDir prefix relative to web path */
    base: "/build/",
    build: {
        manifest: true,
        emptyOutDir: true,
        assetsDir: "",
        outDir: "../public/build/",
        rollupOptions: {
            input: {
              app: "./assets/app.ts"
            },
        },
    },
});
```