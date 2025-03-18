# Versions

*node* version: `22.14.0 (LTS)`

*npm* version: `11.2.0`

*Electron* version: `35.0.2`

*Electron Forge* version: `7.7.0`

Hi everyone,

I am developing an application based on *Forges* `webpack-typescript` template and I am struggeling with the integration of some assets like configuration files, images and fonts. I spend some days on figuring out how to include these files, but I got stuck.

I recognized that there is an import of the default stylesheet `index.css` in the `renderer.ts` file. Both files are located inside the `./src`. But I am not quiete sure, why this is necessary.
I would guess, that this somehow related to the definition of `entryPoints` of the application. Is it correct, that an *Electron* application can define multiple `entryPoints`, which could be denoted as *views*? Is that correct? If it is, than each view needs an *HTML* file and two *TypeScript* files used as the renderer and preload script. The *HTML* file defines the graphical user interface, while the `preload.ts` defines an interface for the inter-process communication between the renderer and the main process. The `renderer.ts` file on the other hand defines the logic for the graphical user interface. The `name` defines the name of the entrypoint. Is that correct?

`forge.config.ts`:
```TypeScript
// ...
renderer: {
    config: rendererConfig,
    entryPoints: [
        {
            html: './src/index.html',
            js: './src/renderer.ts',
            name: 'main_window',
            preload: {
                js: './src/preload.ts',
            },
        },
    ],
},
//...
```

Does *Forge* or *webpack* use this information in order to connect the `renderer.ts` and the *HTML* file? Because the `renderer.ts` is not referenced by the *HTML* file provided by the template. Is this, why the stylesheets need to be imported in the `renderer.ts`? There is an rule 
inside the `webpack.renderer.config.ts` file, provided by the template.

```TypeScript
rules.push({
  test: /\.css$/,
  use: [{ loader: 'style-loader' }, { loader: 'css-loader' }],
});
```

Which role does this rule play in this context? Is this rule necessary to handle the imports in the `renderer.ts` file?

What about other files, like images and fonts? How can I include these files in my application in both developement and producton (build) context? Currently the files I want to include are inside the `./assets` directory. Is the font referenced by the stylesheet automatically resolved?

Should I include an extra rule in the `webpack.renderer.config.ts` file for handling *.svg*, *.png*, *.jpeg* and *.jpg*? Do I need to import these files in the `renderer.ts` as well? I red, that there is a new way of including or loading these kind of assets into the bundle, *webpack* creates. This way, there is should be no need for using `file-loader`, `url-loader` etc. Is that correct? What about this rule? How does this work?

```TypeScript
rules.push({
  test: /\.(jpeg|jpg|png|svg|ico|icns)$/,
  type: 'asset/resource',
});
```
