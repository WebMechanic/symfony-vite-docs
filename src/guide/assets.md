# Vite Assets managements

When you reference assets files in your js or css files, you should remember that you need to use a relative path if you wants Vite process your file.
- **all your files defined with an absolute path will be ignored by Vite** and will be left as is in your build files. You can specify an absolute path relative to your public folder. this practice is not recommended because your asset files will not be versioned.
- **all your files defined with a relative path will be processed by Vite**. The paths are relative to the file where they are referenced. Any assets referenced via a relative path will be re-written, versioned, and bundled by Vite.

## Symfony Asset Component

Whenever you build your Vite app, two configuration files are generated in your output folder (default location: public/build/): manifest.json (generated by vite core), entrypoints.json (generated by vite-plugin-symfony).

The manifest.json file is needed to get the versioned filename of assets files, like font files or image files.

so you can use Symfony's asset component and its asset function to take advantage of this file.
To be able to use this during development you will have to use `ViteAssetVersionStrategy`.

```yaml
# config/packages/framework.yaml
framework:
    assets:
        version_strategy: 'Pentatrion\ViteBundle\Asset\ViteAssetVersionStrategy'

```

You can also use the asset twig function by specifing your asset file path relative to your root path (for compatibility reason with vite generated manifest file) specified in your vite.config.js

```twig
<body>
    <img src="{{ asset('assets/images/avatar.jpg') }}" />
</body>
```

You can use this `asset()` function only with assets referenced by JavaScript or CSS files. If you want to make Vite aware of others assets you can import directory of assets into your application's entry point. For example il you want to version all images stored in `assets/images` you could add the following in your `app` entrypoint.

```
├──assets
│ ├──images
│ │ ├──climbing.jpg
│ │ ├──violin.jpg
│ │ ├──...
│ │
│ ├──app.js
│...
```

```js
// assets/app.js
import.meta.glob([
    './images/**'
]);
```