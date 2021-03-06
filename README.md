libTroxel
======
## **Attention: This Project is still WIP (work in progress) and not ready for use yet.**

LibTroxel is a JavaScript library which allows you to embedd voxel models rendered with [Troxel](https://github.com/chrmoritz/Troxel) into your own website.

## How to use

LibTroxel is licensed under the same license as [Troxel](https://github.com/chrmoritz/Troxel), the [GNU LGPL v3.0](LICENSE.txt) . You can find an [example usage of libTroxel](https://troxeljs.github.io/libtroxel/libTroxelTest.html) and it's [source code here](test/libTroxelTest.html).

#### Install

If you want to use our github pages site as a CDN just add these lines to your html:

```html
<script src="https://troxeljs.github.io/libtroxel/libTroxel.min.js" type="text/javascript"></script>
```

### API

Create somewhere in your layout a `<div>` element and set it's size to the size the rendered model should have. You can add a fallback content inside it, which will be shown if the model is  not available or your users browser doesn't support WebGL.

#### Troxel.renderBlueprint(blueprintId, domElement, [options], [callback])

Renders any Trove blueprint into the given DOM element. It has these parameters:
* `blueprintId` is the id of the blueprint (the filename without the `.blueprint` file extension)
* `domElement` is either a DOM element or a JQuery Object representing this DOM element
* `options` is an optional Object of [render options](#options)
* `callback(error, options)` is an optional callback function which the `error` argument set to `null` if the blueprint is successfully loaded or with `error` set to an Error object if an error has occurred (WebGL not support or blueprint not found ). `options` is an Object with setters and getters containing nearly all [render options](#options)

```JavaScript
Troxel.renderBlueprint('deco_candy_torch_mallow[Laoge]', $('#container'), {
    autoRotate: true,
    autoRotateSpeed: 4.0,
    rendererClearColor: 0x9c9c9c,
    ambientLightColor: 0x707070,
    directionalLightColor: 0xeeeeee,
    directionalLightIntensity: 0.9,
    directionalLightVector: {x: 0.58, y: 0.58, z: 0.58}
}, function(error, resultOptions){
  if (error === null){
    resultOptions.noZoom = true;
  }
});
```

#### Troxel.renderBase64(base64, domElement, [options])

Renders any voxel model represented in Troxel's Base64 format into the given DOM element. It has these parameters:
* `base64` is a Base64 encoded String containing the voxel data of your model (check out Troxels `Link (share)` export options and use the base64 string starting after `#m=`)
* `domElement` is either a DOM element or a JQuery Object representing this DOM element
* `options` is an optional Object of [render options](#options)
Returns an Object with the `error` property set to `null` if it was able to successfully load the model or set to a Error object if WebGL isn't supported or the base64 string was invalid. In the first case a `options` property is defined containing a Object with nearly all [render options](#options) as setters and getters.

#### Troxel.webgl()

Returns `true` if the current browser supports WebGL and `false` otherwise.

#### Markup API (data attributes)

If you can't use Java Script to call the Java Script API (for example in wiki templates or forums bb-tags) you can also embedd voxel models with libTroxel using html markup only. Just create a div with the desired size and these data attributes:
* `data-troxel-blueprint`: set it to the id of the blueprint (the filename without the `.blueprint` file extension)
* or `data-troxel-base64`: set it to a Base64 formated String containing the voxel data of your model (check out Troxels `Link (share)` export options and use the base64 string starting after `#m=`)
* and optionally `data-troxel-options`: set it to a JSON containing a Object of [render options](#options)

```html
<div data-troxel-blueprint="item_tf_candy" data-troxel-options='{"autoRotate": false}' style="width: 300px; height: 300px;">
    <!-- insert fallback content (like a static image of the rendered model) here -->
</div>
```

### Options

`options` is a JavaScript Object with these optional keys:

Options                    | Description                                                                                        | Default
---------------------------|----------------------------------------------------------------------------------------------------|-------------
`autoRotate`               | set it to `true` to automatically rotate around the voxel model                                    | `true`
`autoRotateSpeed`          | the rotation speed in full rotation per minute at 60 fps (set to positive values to change the auto rotate direction, defaults to 15 sec / rotation)                                                                                       | `-4.0`
`renderMode`               | set the render mode (affect color in which each voxel is rendered) as an integer with one of these values: <table><thead><tr><th>value</th><th>description</th></tr></thead><tbody><tr><td>`0`</td><td>**pretty** (default): renderer uses all material maps</td></tr><tr><td>`1`</td><td>renders only the **color** of voxels (no material maps)</td></tr><tr><td>`2`</td><td>renders voxels in the color used in the **alpha** map</td></tr><tr><td>`3`</td><td>renders voxels in the color used in the **type** map</td></tr><tr><td>`4`</td><td>renders voxels in the color used in the **specular** map</td></tr></tbody></table>             | `0`
`renderWireframes`         | set the way wireframes are rendererd as an integer with one of these values: <table><thead><tr><th>value</th><th>description</th></tr></thead><tbody><tr><td>`0`</td><td>**Off** (default): no wireframes are rendered</td></tr><tr><td>`1`</td><td>renders all wireframes in a **darkgrey** color</td></tr><tr><td>`2`</td><td>renders wireframes as defined in the **color** map</td></tr><tr><td>`3`</td><td>renders wireframes as defined in the **alpha** map</td></tr><tr><td>`4`</td><td>renders wireframes as defined in the **type** map</td></tr><tr><td>`4`</td><td>renders wireframes as defined in the **specular** map</td></tr></tbody></table>                                                                               | `0`
`rendererAntialias`        | disables the antialiasing of the renderer if set to `false` *(only available at initialisation)*   | `true`
`rendererSSAO`             | enables the SSAO post effect of the renderer if set to `true` *(only available at initialisation)* | `false`
`rendererClearColor`       | the color of the background behind the voxel model                                                 | `0x888888`
`ambientLightColor`        | the color of the ambient light                                                                     | `0x606060`
`directionalLightColor`    | the color of the directional light                                                                 | `0xffffff`
`directionalLightIntensity`| the intensity of the direction light as a Float                                                    | `0.3`
`directionalLightVector`   | the vector direction of the directional light as an Object *(don't need to be a normal vector)*    | `{x: -0.5, y: -0.5, z: 1}`
`pointLightColor`          | the color of the point light following the camera                                                  | `0xffffff`
`pointLightIntensity`      | the intensity of the point light following the camera as a Float                                   | `0.7`
`controls`                 | set to `true` for Orbital controls or to `false` for Fly controls                                  | `true`
`noRotate`                 | disables the rotate controls if set to `true`                                                      | `false`
`noPan`                    | disables the pan controls if set to `true`                                                         | `false`
`noZoom`                   | disables the zoom controls if set to `true`                                                        | `false`
`showInfoLabel`            | set to `false`, if you want to hide the 'Open this model in Troxel' link *(I would appreciate it if you would link somewhere else to Troxel in this case, this option is only available at initialisation)*                                   | `true`

*Note: for performance reasons you should prefere passing colors as a Javascript hexadecimal Numbers instead of hex strings like in css*

In addition to this there are 2 more options available in the `options` Object returned from `Troxel.renderBase64` or passed in the callback by `Troxel.renderBlueprint` to change the rendered model while reusing the canvas and WebGLContext.

Options     | Description
------------|---------------------------------------------------------------------------------------------------------------------------------
`blueprint` | set it to the id of a valid blueprint (the filename without the `.blueprint` file extension) to render it in the current canvas / WebGLContext (this will only work from a `options` Object passed in the callback of `Troxel.renderBlueprint`)
`base64`    | set it to a valid Base64 encoded String containing the voxel data of your model to render it in the current canvas / WebGLContext (you can also use it to read the `base64` represantation of a voxel model opend with `Troxel.renderBlueprint`)
