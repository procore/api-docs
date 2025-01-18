---
permalink: /bim-web-viewer
title: 'Procore BIM Web Viewer: Integrator Documentation'
layout: default
section_title: BIM Web Viewer
---

<!-- markdownlint-disable no-inline-html -->

## Getting started

<p class="heading-link-container"><a class="heading-link" href="#getting-started"></a></p>

### Installation from NPM (recommended)

<p class="heading-link-container"><a class="heading-link" href="#installation-from-npm-recommended"></a></p>

![npm version](https://img.shields.io/npm/v/@procore/bim-webviewer-sdk?color=%23f47e42)

The Procore BIM Web Viewer is available as a module on `npm` as [`@procore/bim-webviewer-sdk`](https://www.npmjs.com/package/@procore/bim-webviewer-sdk).

To install it, open a terminal window in your project folder and run:

```sh
npm install @procore/bim-webviewer-sdk
```

### Installation from a CDN

<p class="heading-link-container"><a class="heading-link" href="#installation-from-a-cdn"></a></p>

Though not recommended for production use, if you just want to prototype something you can use a tool like [`unpkg`](https://unpkg.com/) to get the module onto a page without the overhead of a build-system. Note we do not control `unpkg` and do not otherwise host the package on a publicly available CDN with uptime guarantees.

```html
<script src="https://unpkg.com/@procore/bim-webviewer-sdk"></script>
```

### Installation by Saving the File

<p class="heading-link-container"><a class="heading-link" href="#installation-by-saving-the-file"></a></p>

If you prefer not to use `npm`, you can download the latest version of the code here: [https://unpkg.com/@procore/bim-webviewer-sdk](https://unpkg.com/@procore/bim-webviewer-sdk)

If you would like a specific version you can declare that version in the url like so: [https://unpkg.com/@procore/bim-webviewer-sdk@4.0.0](https://unpkg.com/@procore/bim-webviewer-sdk@4.0.0)

### Load the Module with a Bundler (recommended)

<p class="heading-link-container"><a class="heading-link" href="#load-the-module-with-a-bundler-recommended"></a></p>

If you're using a bundling system with `import` (recommended):

```js
import * as ProcoreBim from '@procore/bim-webviewer-sdk';
```

### Load the Module Manually

<p class="heading-link-container"><a class="heading-link" href="#load-the-module-manually"></a></p>

If you're managing script loading manually:

```html
<!-- This will instantiate window.ProcoreBim -->
<script src="path/to/Procore.Bim.Webviewer.js"></script>
```

### Instantiate and Start the Viewer

<p class="heading-link-container"><a class="heading-link" href="#instantiate-and-start-the-viewer"></a></p>

```js
const viewer = new ProcoreBim.Webviewer(options);
viewer.start();
```

See the [Options](#options) section for more detail on the options object.

## Background Color

<p class="heading-link-container"><a class="heading-link" href="#background-color"></a></p>

The viewer does not render a background or color.
You may set your own background color with CSS on any parent element that contains the viewer for that background color to be displayed.

## API Introduction

<p class="heading-link-container"><a class="heading-link" href="#api-introduction"></a></p>

When the Webviewer SDK is loaded and parsed on your page, a new global object is added to your window with the key ProcoreBim.
ProcoreBim contains a mix of both static and non static methods and are generally grouped into namespaces to indicate primary functionality.

### ProcoreBim Namespaces

<p class="heading-link-container"><a class="heading-link" href="#procorebim-namespaces"></a></p>

| Namespace | Description                                                                                                    |
| --------- | -------------------------------------------------------------------------------------------------------------- |
| Cache     | Model caching management [Cache Namespace](#cache-namespace)                                                   |
| Webviewer | Model viewer to be instanced and allows for data and camera access [Webviewer Namespace](#webviewer-namespace) |

### Webviewer Namespaces

<p class="heading-link-container"><a class="heading-link" href="#webviewer-namespaces"></a></p>

| Namespace | Description                                                                                   |
| --------- | --------------------------------------------------------------------------------------------- |
| api       | Helper method to retrieve data from procore server [Api Namespace](#api-namespace)            |
| camera    | Webviewer camera retrieval and manipulation [Camera Namespace](#camera-namespace)             |
| dom       | Helper methods to create extendable panels [Dom Namespace](#dom-namespace)                    |
| events    | Event management system similar to Javascript Event API [Events Namespace](#events-namespace) |
| model     | Webviewer model data retrieval and manipulation [Model Namespace](#model-namespace)           |
| markup    | Webviewer markup manipulation [Markup Namespace](#markup-namespace)                           |
| gui       | Webviewer GUI manipulation [GUI Namespace](#gui-namespace)                                    |
| none      | Methods not grouped into a namespace                                                          |

When you create a new instance of Webviewer, an object is returned that allows you to act on the model and camera data.
Acting on these methods is done through dot notation.
Some methods belong to a sub namespace, and some methods do not.

```js
const viewer = new ProcoreBim.Webviewer(options);
viewer.start();
```

The Webviewer has been instanced and we have invoked the `start` method which does not belong to a namespace to start initializing the model viewer.

```js
viewer.camera.setPosition(1, 10, -1);
```

Here we invoke `setPosition` method from the camera namespace to set the camera position in the model viewer.

The `Cache` namespace contains static methods and has the following pattern:

```js
ProcoreBim.Cache.{method_name}
```

#### Complete Example

```js
const viewer = new ProcoreBim.Webviewer(options);

// Starts the viewer, must be called.
// `start` does not belong to a namespace.
viewer.start();

// Gets the camera direction, `getCameraDirection()` belongs to
// the `camera` namespace.
var cameraDirection = viewer.camera.getCameraDirection();

// Checks if the model is cached, `hasModel` returns a promise
// and belongs to the `Cache` namespace.
ProcoreBim.Cache.hasModel({
  meshUrl: 'samples/vortex.mesh',
  meshnodeUrl: 'samples/vortex.meshnode',
  nodeUrl: 'samples/vortex.node',
}).then(function (isCached) {
  console.log(isCached);
});
```

## General API

<p class="heading-link-container"><a class="heading-link" href="#general-api"></a></p>

### Starting the Webviewer

<p class="heading-link-container"><a class="heading-link" href="#starting-the-webviewer"></a></p>

```js
start();
```

#### Description

Entry point to the Webviewer and starts rendering.
This method must be called.

#### Parameters

None

##### Returns

```js
undefined
```

##### Namespace

None

---

Terminating the Webviewer

```js
terminate();
```

#### Description

Stops rendering, frees memory, and removes webviewer from parent element.

#### Parameters

None

##### Returns

```js
undefined
```

##### Namespace

None

## Api Namespace

<p class="heading-link-container"><a class="heading-link" href="#api-namespace"></a></p>

### Get Object Name

<p class="heading-link-container"><a class="heading-link" href="#get-object-name"></a></p>

```js
getNamesForObjects(objectIds);
```

#### Description

Return an array of object names with the given id.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids |

##### Returns

```js
{
  id: number,
  value: string
}
```

##### Namespace

Api

---

### Filter Objects

<p class="heading-link-container"><a class="heading-link" href="#filter-objects"></a></p>

```js
filterObjects(objectIds);
```

#### Description

Lists objects for the loaded model given a filter. The schema for the filter parameter is defined as

```js
export const ObjectFilterSchema: JSONSchemaType<ObjectFilter> = {
  type: 'object',
  properties: {
    filterField: {
      type: 'string',
      enum: ['name'],
    },
    filterValue: {
      type: 'string',
    },
  },
  required: ['filterField', 'filterValue'],
};
```
Currently, we only support a filter field of `name`.

Example usage:
```js
const response = await this.viewer.api.filterObjects({
  filterField: 'name',
  filterValue: 'wall',
});
```

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| filter | true | Object | An ObjectFilterSchema |

##### Returns

```js
Response Object
```

##### Namespace

Api

## Camera Namespace

<p class="heading-link-container"><a class="heading-link" href="#camera-namespace"></a></p>

### Get Camera Position

<p class="heading-link-container"><a class="heading-link" href="#get-camera-position"></a></p>

```js
getPosition();
```

#### Description

Retrieves the active camera position.

#### Parameters

None

##### Returns

```js
Promise<{
  x: Number, y: Number, z: Number
}>
```

##### Namespace

Camera

---

### Set Camera Position

<p class="heading-link-container"><a class="heading-link" href="#set-camera-position"></a></p>

```js
setPosition(x, y, z);
```

#### Description

Sets the camera position.

#### Parameters

| Field Name | Required | Type   | Description                             |
| ---------- | -------- | ------ | --------------------------------------- |
| x          | true     | Number | X component of the world space position |
| y          | true     | Number | Y component of the world space position |
| z          | true     | Number | Z component of the world space position |

#### Returns

```js
Promise<{
  position: {x: Number, y: Number, z: Number},
  direction: {x: Number, y: Number, z: Number}
}>
```

##### Namespace

Camera

---

### Get Camera Direction

<p class="heading-link-container"><a class="heading-link" href="#get-camera-direction"></a></p>

```js
getCameraDirection();
```

#### Description

Retrieves the direction vector for the active camera target.

#### Parameters

None

##### Returns

```js
Promise<{
  x: Number, y: Number, z: Number
}>
```

##### Namespace

Camera

---

### Get Camera Look At

<p class="heading-link-container"><a class="heading-link" href="#get-camera-look-at"></a></p>

```js
getLookAt();
```

#### Description

Gets the camera target.

#### Parameters

None

#### Returns

```js
Promise<{
  x: Number, y: Number, z: Number
}>
```

### Set Camera Look At

<p class="heading-link-container"><a class="heading-link" href="#set-camera-look-at"></a></p>

```js
setLookAt(x, y, z);
```

#### Description

Sets the camera target.

#### Parameters

| Field Name | Required | Type   | Description                             |
| ---------- | -------- | ------ | --------------------------------------- |
| x          | true     | Number | X component of the world space position |
| y          | true     | Number | Y component of the world space position |
| z          | true     | Number | Z component of the world space position |

#### Returns

```js
Promise<{
  position: {x: Number, y: Number, z: Number},
  direction: {x: Number, y: Number, z: Number}
}>
```

##### Namespace

Camera

---

### Get Screen Position

<p class="heading-link-container"><a class="heading-link" href="#get-screen-position"></a></p>

```js
getScreenPosition(x, y, z);
```

#### Description

Take position in world space and convert it to screen space

#### Parameters

| Field Name | Required | Type   | Description                             |
| ---------- | -------- | ------ | --------------------------------------- |
| x          | true     | Number | X component of the world space position |
| y          | true     | Number | Y component of the world space position |
| z          | true     | Number | Z component of the world space position |

##### Returns

```js
Promise<{
  x: Number, y: Number, z: Number
}>
```

##### Namespace

Camera

---

### Get Snapshot

<p class="heading-link-container"><a class="heading-link" href="#get-snapshot"></a></p>

```js
getSnapshot(options);
```

#### Description

Force download the current render view into a png.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `options` | false | `GetSnapshotOptions` | Options to configure the snapshot |

##### `GetSnapshotOptions`

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| `color` | false | `'#ecf2f6'` | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Background color of snapshot |
| `aspectRatio` | false | Width and height of the Web Viewer canvas | `{ width: number; height: number }` | Requested aspect ratio of snapshot. An image will be produced that is the maximum size available from the Web Viewer canvas that matches the requested aspect ratio. This will NOT produce an image with these dimensions unless you happen to request an aspect ratio that is the same dimensions of the Web Viewer canvas. For example, requesting a 100x100 aspect ratio image on a 1600x900 canvas will result in a 900x900 image. |

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Get Snapshot Data URL

<p class="heading-link-container"><a class="heading-link" href="#get-snapshot-data-url"></a></p>

```js
getSnapshotDataUrl(options);
```

#### Description

Returns a data url of an image produced by the current render view.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `options` | false | `GetSnapshotDataUrlOptions` | Options to configure the snapshot |

##### `GetSnapshotDataUrlOptions`

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| `color` | false | `'#ecf2f6'` | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Background color of snapshot |
| `aspectRatio` | false | Width and height of the Web Viewer canvas | `{ width: number; height: number }` | Requested aspect ratio of snapshot. An image will be produced that is the maximum size available from the Web Viewer canvas that matches the requested aspect ratio. This will NOT produce an image with these dimensions unless you happen to request an aspect ratio that is the same dimensions of the Web Viewer canvas. For example, requesting a 100x100 aspect ratio image on a 1600x900 canvas will result in a 900x900 image. |

##### Returns

The canvas as a base64-encoded image.

```js
Promise<string>
```

##### Namespace

Camera

---

### Set Camera From BCF Camera

<p class="heading-link-container"><a class="heading-link" href="#set-camera-from-bcf-camera"></a></p>

```js
setBcfCamera(bcfCamera, applyOffset);
```

#### Description

Sets the camera position and direction from BCF camera data.
See [Orthogonal Camera](#orthogonal-camera-object) or [Perspective Camera](#perspective-camera-object)

NOTE: You should always pass `true` for the `applyOffset` argument. If you pass nothing or `false` then it is probably not what you want. It will soon be removed and that will be the default as it is an implementation detail that need not be exposed. See [migration guide for v7 to v8](#v7-to-v8) for more details.

#### Parameters

| Field Name   | Required | Type    | Description                                 |
| ------------ | -------- | ------- | ------------------------------------------- |
| bcfCamera    | true     | Object  | A BCF formatted object                      |
| applyOffset  | false    | boolean | Whether to apply global offset to bcfCamera |

##### Returns

```js
Promise<{
  perspective_camera: {
    camera_view_point:  {x: Number, y: Number, z: Number},
    camera_direction:  {x: Number, y: Number, z: Number},
    camera_up_vector:  {x: Number, y: Number, z: Number},
    field_of_view: Number,
    target_distance: Number
  }
}>
```

##### Namespace

Camera

---

### Get BCF Camera From Camera

<p class="heading-link-container"><a class="heading-link" href="#get-bcf-camera-from-camera"></a></p>

```js
getBcfCamera();
```

#### Description

Retrieves a BCF perspective camera.

#### Parameters

None

##### Returns

```js
Promise<{
  perspective_camera: {
    camera_view_point:  {x: Number, y: Number, z: Number},
    camera_direction:  {x: Number, y: Number, z: Number},
    camera_up_vector:  {x: Number, y: Number, z: Number},
    field_of_view: Number,
    target_distance: Number
  }
}>
```

##### Namespace

Camera

---

### Navigation to the home viewpoint

<p class="heading-link-container"><a class="heading-link" href="#navigation-to-the-home-viewpoint"></a></p>

```js
navToHomeView();
```

#### Description

Navigation to the home viewpoint if `bcfCamera` option has been set. If not, zooms to fit the axis-aligned bounding box of the entire model with space junk eliminated (see [`zoomToGlobal`](#zoom-to-global)).

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Zoom to Objects

<p class="heading-link-container"><a class="heading-link" href="zoom-to-objects"></a></p>

```js
zoomToObjects(objectIds);
```

#### Description

Moves camera from current position to fit the axis-aligned bounding box of the node or set of objects.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | ObjectId[] | An array of object ids. To zoom to a single object, pass an array of one. |

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Zoom to Fit a Bounding Box

<p class="heading-link-container"><a class="heading-link" href="zoom-to-fit-a-bounding-box"></a></p>

```js
zoomToBoundingBox(bbox);
```

#### Description

Moves camera to fit the specified axis-aligned bounding box.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| bbox | true | BoundingBox | `{ min: { x: Number, y: Number, z: Number }, max: { x: Number, y: Number, z: Number } }` |

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Zoom to Selection

<p class="heading-link-container"><a class="heading-link" href="zoom-to-selection"></a></p>

```js
zoomToSelection();
```

#### Description

Moves camera from current position to fit the axis-aligned bounding box of the current selection.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Zoom to Global

<p class="heading-link-container"><a class="heading-link" href="zoom-to-global"></a></p>

```js
zoomToGlobal();
```

#### Description

Moves camera to fit the axis-aligned bounding box of the entire model. Does not take into account current camera position, i.e. will always give you the same view regardless of where the camera is currently.

As of v5.1.0, `zoomToGlobal` will zoom to the "refined global bounding box" in which "space junk" has been eliminated. Space junk is loosely defined as objects which are far from all other objects in the model and distort the global bounding box dramatically.

Performance Note: the execution time for eliminating space junk scales with the number of meshnodes in the model. Mostly this cost is incurred at load time, but will be incurred the first time `zoomToGlobal` is called if a `bcfCamera` option was provided.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Set Euler Angles

<p class="heading-link-container"><a class="heading-link" href="#set-euler-angles"></a></p>

```js
setEulerAngles(x, y, z);
```

#### Description

Sets the camera with euler angles, yaw, pitch and roll

#### Parameters

| Field Name | Required | Type   | Description                    |
| ---------- | -------- | ------ | ------------------------------ |
| x          | true     | Number | Angle of the x axis in radians |
| y          | true     | Number | Angle of the y axis in radians |
| z          | true     | Number | Angle of the z axis in radians |

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

### Set Markup

<p class="heading-link-container"><a class="heading-link" href="#set-markup"></a></p>

```js
setMarkup(markupData, fov);
```

#### Description

This is a deprecated method. Use [`markup.setRedlines`](#set-redlines), which has the same signature.

---

### Set Pose

<p class="heading-link-container"><a class="heading-link" href="#set-pose"></a></p>

```js
setPose(params);
```

#### Description

Sets the position and orientation of a 3D camera based on a given pose.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| params | true | object | An object containing the properties `pose` and `cameraUp`. |

##### Params Object

The `params` object has the following structure:

| Name | Type |	Description |
| pose | String or Object |	The pose to set for the camera. This can either be a string representing a predefined pose, or an object containing `position` and `lookAt` properties. |
| cameraUp | Object (optional) | An object representing the up direction for the camera, with `x`,`y`, and `z` properties. If not already normalized, will be normalized for you and set the camera to this up direction. |

If `pose` is an object, it should have the following structure:

| Name | Type |	Description |
| position |	Object |	An object representing the position of the camera, with `x`, `y`, and `z` properties. |
| lookAt	| Object |	An object representing the point that the camera should look at, with `x`, `y`, and `z` properties. |

If pose is a string, it should be one of the following values: "top", "bottom", "front", "back", "left", "right"

###### Example

```javascript
camera.setPose({
  pose: {
    position: { x: 1, y: 2, z: 3 },
    lookAt: { x: 4, y: 5, z: 6 },
  },
  cameraUp: { x: 0, y: 1, z: 0 },
});
```

In this example, the `setPose` method sets the camera's position to (1, 2, 3), sets the camera to look at the point (4, 5, 6), and sets the camera's up direction to point in the positive y-direction.

```javascript
camera.setPose({
  pose: "top",
});
```
In this example, the `setPose` method sets the camera to a predefined "top" pose, which sets the position of the camera above the model and looks down at it.

##### Returns

```js
Promise<void>
```

##### Namespace

Camera

---

## DOM Namespace

<p class="heading-link-container"><a class="heading-link" href="#dom-namespace"></a></p>

### Add Panel

<p class="heading-link-container"><a class="heading-link" href="#add-panel"></a></p>

```js
addPanel(domElement);
```

#### Description

Adds a container div to the Webviewer canvas and appends `Element` to it.

#### Parameters

| Field Name | Required | Type        | Description                                                           |
| ---------- | -------- | ----------- | --------------------------------------------------------------------- |
| domElement | true     | HTMLElement | The DOM element that makes up the panel to be appended to the viewer. |

##### Returns

```js
boolean
```

##### Namespace

Dom

---

### Remove Panel

<p class="heading-link-container"><a class="heading-link" href="#remove-panel"></a></p>

```js
removePanel(domElement);
```

#### Description

Removes the container div from the viewer which contains the argument `Element`.

#### Parameters

| Field Name | Required | Type        | Description                                                            |
| ---------- | -------- | ----------- | ---------------------------------------------------------------------- |
| domElement | true     | HTMLElement | The DOM element that makes up the panel to be removed from the viewer. |

##### Returns

```js
boolean
```

##### Namespace

Dom

## Events Namespace

<p class="heading-link-container"><a class="heading-link" href="#events-namespace"></a></p>

### Add Event

<p class="heading-link-container"><a class="heading-link" href="#add-event"></a></p>

```js
addEventListener(eventName, callback);
```

#### Description

Executes a callback when an internal viewer event occurs.

The `callback` parameter is a third party defined function to be executed when the event fires. Depending on the event, the callback function may return undefined, or data. This data may be a number, string, or a JavaScript object.

#### Parameters

| Field Name | Required | Type     | Description                         |
| ---------- | -------- | -------- | ----------------------------------- |
| eventName  | true     | String   | Name of the event                   |
| callback   | true     | Function | Function to evoke when event occurs |

##### Returns

```js
void
```

##### Namespace

Events

---

### Remove Event

<p class="heading-link-container"><a class="heading-link" href="#remove-event"></a></p>

```js
removeEventListener(eventName, callback);
```

#### Description

Removes a callback from an internal viewer event, if it is identical to a callback that was already added.

#### Parameters

| Field Name | Required | Type     | Description                                         |
| ---------- | -------- | -------- | --------------------------------------------------- |
| eventName  | true     | String   | Name of the event                                   |
| callback   | true     | Function | The function that was added from `addEventListener` |

##### Returns

```js
void
```

##### Namespace

Events

---

### Dispatch Event

<p class="heading-link-container"><a class="heading-link" href="#dispatch-event"></a></p>

```js
dispatchEvent(eventName, callback);
```

#### Description

Manually dispatches an event

#### Parameters

| Field Name | Required | Type     | Description                                         |
| ---------- | -------- | -------- | --------------------------------------------------- |
| eventName  | true     | String   | Name of the event                                   |
| callback   | true     | Function | The function that was added from `addEventListener` |

##### Returns

```js
void
```

##### Namespace

Events

---

## Event Names

<p class="heading-link-container"><a class="heading-link" href="#event-names"></a></p>

The following event names are available to notify third party code of changes in the viewer.

#### Complete Example

```js
const viewer = new ProcoreBim.Webviewer(options);

viewer.events.addEventListener('appResize', (data) => {
  // Called when the window resizes and returns an object with properties
  // `offsetHeight`, `offsetWidth`, `offsetLeft`, `offsetTop`.
  console.log(data.offsetHeight);
});

viewer.events.addEventListener('bcfCameraSet', () => {
  // Called when the camera has been set, does not return any data.
});

viewer.start();
```

### animate

<p class="heading-link-container"><a class="heading-link" href="#animate"></a></p>

Fires at the end of the animation loop. Returns an object containing the property `stage` to indicate the stage the renderer is in to render the scene. `renderIdle` indicates that the scene has been fully rendered and the renderer is no longer rendering any objects. `renderStart` indicates the start of rendering a scene. `renderProgress` is a state after `renderStart` and indicates the renderer is still rendering the scene. Finally, `renderComplete` indicates that all the objects have been rendered.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| stage | string | The stage the renderer is in during the animation loop. Can either be `renderIdle`, `renderStart`, `renderProgress`, or `renderComplete`. |

---

### appResize

<p class="heading-link-container"><a class="heading-link" href="#appresize"></a></p>

Fires when the document view (window) has been resized.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| offsetHeight | number | New offset height of the webviewer parent element. |
| offsetWidth  | number | New offset width of the webviewer parent element.  |
| offsetLeft   | number | New offset left of the webviewer parent element.   |
| offsetTop    | number | New offset top of the webviewer parent element.    |

---

### appStart

<p class="heading-link-container"><a class="heading-link" href="#appstart"></a></p>

Fires after `start()` is called on the Webviewer instance.

---

### bcfCameraSet

<p class="heading-link-container"><a class="heading-link" href="#bcfcameraset"></a></p>

Fires after a BCF Camera has been set.

---

### cameraUpdated

<p class="heading-link-container"><a class="heading-link" href="#cameraupdated"></a></p>

Fires after the camera direction or camera position has been modified.

---

### canvasReady

<p class="heading-link-container"><a class="heading-link" href="#canvasready"></a></p>

Fires after the rendering canvas has been created.

---

### coachmarkHyperlinkClicked

<p class="heading-link-container"><a class="heading-link" href="#coachmarkhyperlinkclicked"></a></p>

Fires when the coachmark indicator has been cleared. The coachmark will appear in the upper right hand corner of the webviewer appear when:
- objects are hidden
- section planes are applied
- a field of view has been applied other than the default field of view

When the coachmark is cleared, this event will fire with a value indicating which type of coachmark was cleared:
- `"hidden"`
- `"section"`
- `"fov"`

---

### doubleClick

<p class="heading-link-container"><a class="heading-link" href="#doubleclick"></a></p>

Fires after a double click occurs on the webviewer container element. Returns a [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent) object.

---

### downloadComplete

<p class="heading-link-container"><a class="heading-link" href="#downloadcomplete"></a></p>

Fires after all download requests for the model have been completed. This event will still fire even if the model is cached.

---

### downloadProgress

<p class="heading-link-container"><a class="heading-link" href="#downloadprogress"></a></p>

Fires periodically to report progress on how much of the model has been downloaded.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| loaded      | number  | Number of bytes downloaded.                                                                                     |
| total       | number  | Total size of model in bytes. This amount will be arbitrarily large until all the requests have been initiated. |
| total_found | boolean | Returns true to indicates all the requests for the model has been initiated.                                    |

---

### downloadStart

<p class="heading-link-container"><a class="heading-link" href="#downloadstart"></a></p>

Fires before requests are made to download the model.

---

### drawingNavigated

<p class="heading-link-container"><a class="heading-link" href="#drawingnavigated"></a></p>

Fires when the 2D Navigation overlay is used to navigate the model. Specifically, this fires after a location and direction have been chosen on the floor plan and the overlay is dismissed.

---

### drawingMiniClick

<p class="heading-link-container"><a class="heading-link" href="#drawingminiclick"></a></p>

Fires when the 2D Navigation mini map was clicked on.

---

### floorPlan

<p class="heading-link-container"><a class="heading-link" href="#floorplan"></a></p>

Fires when the 2D Navigation overlay is interacted with.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| floorPlanNavigateAndClose | function    | Provides a callback function to navigate the camera to a position and close the overlay. Returns a promise. |
| floorPlanPoint            | function    | Provides a callback function to calculate a point on the current floor plan. Returns a promise.             |
| overlay                   | HTMLElement | The HTMLElement for the 2D Navigation overlay                                                               |
| type                      | string      | The type of interaction and has the following values: `open`, `close`, and `update`                         |

#### floorPlanNavigateAndClose

<p class="heading-link-container"><a class="heading-link" href="#floorplannavigateandclose"></a></p>

Updates the camera position and direction in addition to dismissing the 2D floor plan overlay.

##### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| x          | true     | number | x coordinate in 3D space.  |
| y          | true     | number | y coordinate in 3D space.  |
| z          | true     | number | z coordinate in 3D space.  |
| dx         | true     | number | x direction of the camera. |
| dy         | true     | number | y direction of the camera. |

#### floorPlanPoint

<p class="heading-link-container"><a class="heading-link" href="#floorplanpoint"></a></p>

Returns the corresponding point on the 2D floor plan overlay from a point in 3D model space.

##### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| x          | true     | number | x coordinate of the camera. |
| y          | true     | number | y coordinate of the camera. |
| z          | true     | number | z coordinate of the camera. |
| dx         | true     | number | x direction of the camera.  |
| dy         | true     | number | y direction of the camera.  |

#### Example

```js
const viewer = new ProcoreBim.Webviewer(options);

let floorPlanNavigateAndClose = null;

viewer.events.addEventListener('floorPlan', (data) => {
  // Executed when the 2D floor plan overlay is interacted with.
  if (data.type === 'open') {
    // Your application might need to know when the 2D floor plan overlay is `open` and keep a reference to
    // floorPlanNavigateAndClose
    floorPlanNavigateAndClose = data.floorPlanNavigateAndClose;
  }
});
viewer.start();

// Perhaps your application places markers that represent 3D positions on the 2D floor plan overlay while it is open. When
// tapping on a marker, this function could be called that uses `floorPlanNavigateAndClose` to set the camera and close the
// 2D floor plan overlay.
const goto = (camera) => {
  if (floorPlanNavigateAndClose !== null) {
    floorPlanNavigateAndClose(camera.x, camera.y, camera.z, 0.1, 0.1);
    floorPlanNavigateAndClose = null;
  }
};
```

---

### fieldOfViewChanged

<p class="heading-link-container"><a class="heading-link" href="#fieldofviewchanged"></a></p>

Fires when the camera field of view has been changed. Returns a number in degrees.

---

### globalBoundingBoxInitialized

<p class="heading-link-container"><a class="heading-link" href="#globalboundingboxinitialized"></a></p>

Fires when the global bounding box of the model has been determined.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| bbox | BoundingBox | `{ min: { x: Number, y: Number, z: Number }, max: { x: Number, y: Number, z: Number } }` |

### hideUpdated

<p class="heading-link-container"><a class="heading-link" href="#hideupdated"></a></p>

Fires when the object hidden set has been updated.

---

### hideSimilarCompleted

<p class="heading-link-container"><a class="heading-link" href="#hidesimilarcompleted"></a></p>

Fires when the `Hide Similar` from the right click context menu operation has completed.

---

### intersectPointClick

<p class="heading-link-container"><a class="heading-link" href="#intersectpointclick"></a></p>

Fires when a mouse click occurs on any part of the model and returns that point of intersection.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| x          | number | x coordinate of the point of intersection. |
| y          | number | y coordinate of the point of intersection. |
| z          | number | z coordinate of the point of intersection. |

---

### isolateCompleted

<p class="heading-link-container"><a class="heading-link" href="#isolatecompleted"></a></p>

Fires when the `Isolate` from the right click context menu operation has completed.

---

### messages

<p class="heading-link-container"><a class="heading-link" href="#messages"></a></p>

Fires when the webviewer throws a warning or error.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| messageType | string | The type of message, either `Warning` or `Error`. |
| component   | string | Where the message originates from.                |
| message     | string | A human readable message.                         |

---

### measured

<p class="heading-link-container"><a class="heading-link" href="#measured"></a></p>

Fires when measuring two objects has completed.

---

### modelTreeParentInteracted

<p class="heading-link-container"><a class="heading-link" href="#modeltreeparentinteracted"></a></p>

Fires when an object in the `Model Objects` has been interacted with.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| children | number | The number of children the interacted object has. |
| action | string | The type of action, can be the following: `checkbox`, `open`, `click`, `double_click` or `close` |
| depth | number | The depth of the object interacted with. A depth of 0 indicates top level objects. A depth of 1 indicates an object that is one level down from root level objects and so on. |

---

### markupDisplayed

<p class="heading-link-container"><a class="heading-link" href="#markupdisplayed"></a></p>

Fires when markup from a viewpoint has been displayed.

---

### markupDismissed

<p class="heading-link-container"><a class="heading-link" href="#markupdismissed"></a></p>

Fires when markup from a viewpoint has been dismissed.

---

### navigationChanged

<p class="heading-link-container"><a class="heading-link" href="#navigationchanged"></a></p>

Fires when the navigation mode has changed.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| mode       | number | The type of navigation that was just activated. |
| controls   | object | An instance of the controller object.           |

---

### objectDoubleClick

<p class="heading-link-container"><a class="heading-link" href="#objectdoubleclick"></a></p>

Fires on double mouse clicks. If the double mouse click occurs on an object, this event returns a Meshnode Index, otherwise -1 is returned. A double click also produces one `objectSingleClick` event.

---

### objectSingleClick

<p class="heading-link-container"><a class="heading-link" href="#objectsingleclick"></a></p>

Fires on a single mouse click.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| objectId | number | The object id of the selected object. -1 if empty space is clicked. |
| selectionContainerId | number | The object id of the selection container. Currently this is always the "First Object". -1 if empty space is clicked |
| ancestry | object[] | An array of objects from the root object down to the selected object. Each object also contains fully materialized children one level deep. Past this level the children are object ids. Empty array if empty space is clicked. |


---

### objectHide

<p class="heading-link-container"><a class="heading-link" href="#objecthide"></a></p>

Fires when objects have been hidden.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| origin     | string | The origin of the hide operation, can be: `context_menu`, `keyboard`, `object_tree`, `object_tree_context_menu_hide`, or `object_tree_actions_hide` |

---

### objectUnhide

<p class="heading-link-container"><a class="heading-link" href="#objectunhide"></a></p>

Fires when objects have been removed from the hidden set.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| origin     | string | The origin of the un hide operation, can be: `coachmark_hidden`, `object_tree`, `object_tree_all_objects` |

---

### objectSelect

<p class="heading-link-container"><a class="heading-link" href="#objectselect"></a></p>

Fires when an object has been selected. Does not fire if empty space is clicked because nothing has been selected.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| objectId | number | The object id of the selected object |
| selectionContainerId | number | The object id of the selection container. Currently this is always the "First Object". |
| ancestry | object[] | An array of objects from the root object down to the selected object. Each object also contains fully materialized children one level deep. Past this level the children are object ids |

---

### objectDeselect

<p class="heading-link-container"><a class="heading-link" href="#objectdeselect"></a></p>

Fires when an object has been deselected. This event returns an array of object ids that were deselected.

---

### optionsChanged

<p class="heading-link-container"><a class="heading-link" href="#optionschanged"></a></p>

Fires when the Webviewer options have changed. This event fires when `setOptions` is called, or when actions cause the multiple selection option to change, such as keyboard shortcuts to multi select objects.

---

### objectMeasure

<p class="heading-link-container"><a class="heading-link" href="#objectmeasure"></a></p>

Fires when objects are selected for measurement. This event returns a Meshnode Index of the object selected for measurement.

---

### projectSettingsUpdated

<p class="heading-link-container"><a class="heading-link" href="#projectsettingsupdated"></a></p>

Fires when the project settings have been updated.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| displayUnits | string | An abbreviated string name of the display units used. Possible values are `ftin` for Feet Inches, `mm` for Millimeters, and `m` for Meters. |

---

### projectSettingsLoaded

<p class="heading-link-container"><a class="heading-link" href="#projectsettingsloaded"></a></p>

Fires when the project settings have been loaded.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| displayUnits | string | An abbreviated string name of the display units used. Possible values are `ftin` for Feet Inches, `mm` for Millimeters, and `m` for Meters. |

---

### renderReady

<p class="heading-link-container"><a class="heading-link" href="#renderready"></a></p>

Fires when the webviewer is ready to render the model.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| offsetHeight | number | Offset height of the parent element container. |
| offsetLeft   | object | Offset left of the parent element container.   |
| offsetTop    | number | Offset top of the parent element container.    |
| offsetWidth  | object | Offset width of the parent element container.  |

---

### redo

<p class="heading-link-container"><a class="heading-link" href="#redo"></a></p>

Fires when the keyboard shortcut to redo is pressed. This event fires regardless if there is anything to redo.

---

### selectTool

<p class="heading-link-container"><a class="heading-link" href="#selecttool"></a></p>

Fires when a tool has been activated.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| origin     | string | The origin of the tool being activated.                                                     |
| tool       | number | The tool that has been activated, enumerated from `ProcoreBim.Webviewer.toolbar_enum`. |

---

### selectedUpdated

<p class="heading-link-container"><a class="heading-link" href="#selectedupdated"></a></p>

Fires when the selection set of objects has been updated. This event returns an array of the full set of selected object ids at the time of selection update.

---

### singleClick

<p class="heading-link-container"><a class="heading-link" href="#singleclick"></a></p>

Fires when a single click occurs in the webviewer container element. This event returns a `MouseEvent` object.

---

### sectionBoxDisplayConfigured

<p class="heading-link-container"><a class="heading-link" href="#sectionboxdisplayconfigured"></a></p>

Fires when the section box display is configured (e.g. via [`model.configureSectionBoxDisplay`](#configure-section-box-display)).

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| current | SectionBoxDisplayConfiguration | The configuration that has now taken effect. See [`model.configureSectionBoxDisplay`](#configure-section-box-display)'s params for shape of configuration. |
| previous | SectionBoxDisplayConfiguration | The configuration before the change that fired this event. See [`model.configureSectionBoxDisplay`](#configure-section-box-display)'s params for shape of configuration. |
| enabled | boolean | Whether the section box display is enabled or not |

---

### sectionBoxDisplayToggled

<p class="heading-link-container"><a class="heading-link" href="#sectionboxdisplaytoggled"></a></p>

Fires when the section box display is toggled on or off (e.g. via [`model.toggleSectionBoxDisplay`](#toggle-section-box-display)).

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| configuration | SectionBoxDisplayConfiguration | The configuration that was used when the section box display was toggled on. See [`model.configureSectionBoxDisplay`](#configure-section-box-display)'s params for shape of configuration. |
| value | boolean | Whether the section box display was enabled or disabled |

---

### sectionBoxSet

<p class="heading-link-container"><a class="heading-link" href="#sectionboxset"></a></p>

Fires when a section box has been applied.

---

### sectionPlaneAdded

<p class="heading-link-container"><a class="heading-link" href="#sectionplaneadded"></a></p>

Fires when section planes have been applied.

---

### sectionPlaneRemoved

<p class="heading-link-container"><a class="heading-link" href="#sectionplaneremoved"></a></p>

Fires when a section plane has been removed.

---

### sectionsCleared

<p class="heading-link-container"><a class="heading-link" href="#sectionscleared"></a></p>

Fires when all section planes have been removed as a result of individually removing all the planes, calling `clearSection`, or calling `removeSectionBox`.

---

### savedViewpointClicked

<p class="heading-link-container"><a class="heading-link" href="#savedviewpointclicked"></a></p>

Fires when a Saved Viewpoint from the Viewpoints tool has been clicked on.

---

### undo

<p class="heading-link-container"><a class="heading-link" href="#undo"></a></p>

Fires when the keyboard shortcut to undo is pressed. This event fires regardless if there is anything to undo.

---

### windowComponentChanged

<p class="heading-link-container"><a class="heading-link" href="#windowcomponentchanged"></a></p>

Fires when a window based tool has changed its position or size.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| modalSize  | object | An object with the keys, `height`, `left`, `top`, `width` to indicate the current position and size of the window. |
| name       | string | Human readable name of the window based tool that is changing.                                                     |

---

### windowOpen

<p class="heading-link-container"><a class="heading-link" href="#windowopen"></a></p>

Fires when a window based tool has been opened.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| elementId | string | The value of the id attribute on the root DOM element of the window. |
| toolId | number | The id of the tool the window belongs to. See [Tools](#tools) for further information. |


---

### windowClose

<p class="heading-link-container"><a class="heading-link" href="#windowclose"></a></p>

Fires when a window based tool has been hidden.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| elementId | string | The value of the id attribute on the root DOM element of the window. |
| toolId | number | The id of the tool the window belongs to. See [Tools](#tools) for further information. |

---

### objectTreeUpdated

<p class="heading-link-container"><a class="heading-link" href="#objecttreeupdated"></a></p>

Fires when a filter has been applied to the Object Tree or if the filtered has been cleared. When the filter is cleared, this event returns a null payload.

#### Data Properties

| Field Name | Type | Description |
| - | - | - |
| ancestors | Array[Number] | An aggregate of all object ids that are ancestors of object ids that constructed the filtered object tree. |
| children | Array[Number] | An aggregate of all the object ids that are children of the object ids that constructed the filtered object tree. |
| nodeIds | Array[Number] | An aggregate of all the object ids that constructed the filtered object tree. |

## Model Namespace

<p class="heading-link-container"><a class="heading-link" href="#model-namespace"></a></p>

### Get Meshnode from Meshnode Index

<p class="heading-link-container"><a class="heading-link" href="#get-meshnode-from-meshnode-index"></a></p>

```js
getMeshnode(meshnodeIndex);
```

#### Description

Retrieves a JavaScript object that describes the object.

#### Parameters

| Field Name | Required | Type | Description      |
| - | - | - | - |
| meshnodeIndex | true | Number | meshnode index |

##### Returns

```js
Promise<Meshnode | null> 
```

See [Meshnode](#meshnode).  

##### Namespace

Model

---

### Get Meshnode from Object ID

<p class="heading-link-container"><a class="heading-link" href="#get-object-from-object-id"></a></p>

```js
getMeshnodeFromObjectId(objectId);
```

#### Description

Retrieves the Meshnode associated with an `objectId` or `null` if there is none.

`objectId`'s are returned by these Rest endpoints:

- BIM Property File Objects: https://developers.procore.com/reference/rest/v1/bim-model-objects?version=1.0
- BIM Model Objects: https://developers.procore.com/reference/rest/v1/bim-model-objects?version=1.0

#### Parameters

| Field Name | Required | Type | Description      |
| - | - | - | - |
| objectId | true | Number | Object ID |

##### Returns

```js
Promise<Meshnode>
```

See [Meshnode](#meshnode).

##### Namespace

Model

---

### Get Hidden Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#get-hidden-meshnode-indices"></a></p>

```js
getHiddenMeshnodeIndices();
```

#### Description

Retrieves a list of meshnode indices that are currently hidden.

#### Parameters

None

##### Returns

```js
Promise<number[]>
```

##### Namespace

Model

---

### Add Hidden Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#add-hidden-meshnode-indices"></a></p>

```js
addHiddenMeshnodeIndices(meshnodeIndices);
```

#### Description

Hides meshnode indices. Does not clear previous hiding(s).

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| meshnodeIndices | true | Array[Number] | Array of meshnode indices |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Has Hidden Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#has-hidden-meshnode-indices"></a></p>

```js
hasHiddenMeshnodeIndices(meshnodeIndices);
```

#### Description

Checks if passed meshnode indices are hidden.
Returns `true` is all the meshnode indices in the array are hidden.
Otherwise `false` is returned if at least one meshnode is not hidden.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| meshnodeIndices | true | Array[Number] | Array of meshnode indices |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Clear Hidden Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#clear-hidden-meshnode-indices"></a></p>

```js
clearHiddenMeshnodeIndices();
```

#### Description

Clear hidden meshnode indices.

#### Parameters

None

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Get Selected Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#get-selected-meshnode-indices"></a></p>

```js
getSelectedMeshnodeIndices();
```

#### Description

Retrieves a list of meshnode indices that are currently selected.

#### Parameters

None

##### Returns

```js
Promise<number[]>
```

##### Namespace

Model

---

### Add Selected Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#add-selected-meshnode-indices"></a></p>

```js
addSelectedMeshnodeIndices(meshnodeIndices);
```

#### Description

Selects meshnode indices. Does not clear previous selection(s).

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| meshnodeIndices | true | Array[Number] | Array of meshnode indices |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Has Selected Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#has-selected-meshnode-indices"></a></p>

```js
hasSelectedMeshnodeIndices(meshnodeIndices);
```

#### Description

Checks if passed meshnode indices are selected.
Returns `true` is all the meshnode indices in the array are selected.
Otherwise `false` is returned if at least one meshnode index is not selected.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| meshnodeIndices | true | Array[Number] | Array of meshnode indices |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Clear Selected Meshnode Indices

<p class="heading-link-container"><a class="heading-link" href="#clear-selected-meshnode-indices"></a></p>

```js
clearSelectedMeshnodeIndices();
```

#### Description

Deselects all meshnodes.

#### Parameters

None

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Set Meshnode Color

<p class="heading-link-container"><a class="heading-link" href="#set-meshnode-color"></a></p>

```js
setMeshnodeColor(meshnodeIndex, hexColor, opacity);
```

#### Description

Overrides the color of a meshnode.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| meshnodeIndex | true | Number | Meshnode Index |
| hexColor | true | String | Hex Color |
| opacity | true | Number | Opacity |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Set Object Color

<p class="heading-link-container"><a class="heading-link" href="#set-object-color"></a></p>

```js
setObjectColor(colorDescriptor);
```

#### Description

Changes the color of an object while in three modes; `default`, `selected` or `xray`. Default is while the object is neither selected or in X Ray mode. Selected is while the object is selected. X Ray is while in X Ray mode.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| palettes | true | Object[] | see [colorDescriptor](#colordescriptor) |

##### ColorDescriptor

<p class="heading-link-container"><a class="heading-link" href="#colordescriptor"></a></p>

A ColorDescriptor is composed of a palette, which may contain at least one of default, xray, or selected, and an array of object ids to apply it to.

```js
{
  palette: {
    default: {color: '#ff0000', opacity: 1},
    xray: {color: '#00ff00', opacity: 1},
    selected: {color: '#0000ff', opacity: 1}
  },
  objectIds: [1, 2, 3]
}
```

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Clear Object Color

<p class="heading-link-container"><a class="heading-link" href="#clear-object-color"></a></p>

```js
clearObjectColor(objectIds);
```

#### Description

Clears object colors set by [setObjectColor](#set-object-color) defined by sets of object ids.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | Array of object ids |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Clear All Object Color

<p class="heading-link-container"><a class="heading-link" href="#clear-all-object-color"></a></p>

```js
clearAllObjectColor();
```

#### Description

Clears all object colors set by [setObjectColor](#set-object-color).

#### Parameters

None

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Get Sections

<p class="heading-link-container"><a class="heading-link" href="#get-sections"></a></p>

```js
getSections(format);
```

#### Description

Returns the current sectioning information as set by the Section Box tool or the sectioning API methods (e.g. [`setSectionBox`](#set-section-box) and [`addSectionPlane`](#add-section-plane)).


#### Parameters

| Field Name | Required | Type   | Description         |
| ---------- | -------- | ------ | --------------------|
| format     | true     | string | 'autodesk' \| 'bcf' |

##### Returns

The `format` parameter determines the shape of this data.

The `bcf` format will return an array of [BCF clipping plane](#bcf-clipping-plane). If no section is set it will return `[]`.

The `autodesk` format will return an [Autodesk Section Data object](#autodesk-section-data). If no section is set it will return `null`.

```js
Promise<SectionDataProcoreFormat | SectionDataAutodeskFormat | null>
```

##### Namespace

Model

---

### Set Section Box

<p class="heading-link-container"><a class="heading-link" href="#set-section-box"></a></p>

```js
setSectionBox(minXYZ, maxXYZ, rotation, showCoachmark);
```

#### Description

Sets section box given an XYZ max and min and rotation(optional);

#### Parameters

| Field Name    | Required | Type    | Description                         |
| ------------- | -------- | ------- | ----------------------------------- |
| minXYZ        | true     | Object  | { x: Number, y: Number, z: Number } |
| maxXYZ        | true     | Object  | { x: Number, y: Number, z: Number } |
| rotation      | false    | Object  | { x: Number, y: Number, z: Number } |
| showCoachmark | false    | boolean | Determines whether the Sectioning Applied coachmark is shown. Defaults to true. |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Remove Section Box

<p class="heading-link-container"><a class="heading-link" href="#remove-section-box"></a></p>

```js
removeSectionBox();
```

#### Description

Removes section box.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Reset Section Box

<p class="heading-link-container"><a class="heading-link" href="#reset-section-box"></a></p>

```js
resetSectionBox();
```

#### Description

Resets section box to global bounding box of model.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Toggle Section Box Display

<p class="heading-link-container"><a class="heading-link" href="#toggle-section-box-display"></a></p>

```js
toggleSectionBoxDisplay(enable, options);
```

#### Description

Sets the on/off status of the section box display.

The section box display is the UI widget that allows users to interactively set the section box.

When turned on it will take the size of the first that is defined:

- the size of the last set section box in this session (e.g. via a viewpoint or SDK methods such as [`model.setSectionBox`](#set-section-box))
- the size of the last set section box display in this session (i.e. if the user has enabled the section box display at least once)
- the global bounding box of the model

When turned off it will clear all sections.

If the section box is set while the display is toggled on, the display will also change size to match the section box.

##### Overriding Section Box Setup

If you'd like to use your own logic for applying/removing the actual section box, you can configure `model.toggleSectionBoxDisplay` to use the size of the currently applied section box with the `overrideSectionBoxSetup` option.

This can also be configured for all calls with [`model.configureSectionBoxDisplay`](#configure-section-box-display).

When `overrideSectionBoxSetup` is `true`, you must set a section box manually before calling `model.toggleSectionBoxDisplay`.

```ts
// Starting with no section applied
model.clearSection();

// BAD - throws error because no applied section box to take size of
model.toggleSectionBoxDisplay(true, { overrideSectionBoxSetup: true });

// GOOD - section box display takes size of applied section box
model.setSectionBox({ x: 0, y: 0, z: 0 }, { x: 1, y: 1, z: 1 }, { x: 0, y: 0, z: 0 }, false);
model.toggleSectionBoxDisplay(true, { overrideSectionBoxSetup: true });

// GOOD - using global config
model.configureSectionBoxDisplay({ overrideSectionBoxSetup: true });
model.setSectionBox({ x: 0, y: 0, z: 0 }, { x: 1, y: 1, z: 1 }, { x: 0, y: 0, z: 0 }, false);
model.toggleSectionBoxDisplay(true);
```

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `enable` | true | `boolean` | on/off status to set the section box display to |
| `options` | false | `ToggleSectionBoxDisplayOptions` | Options that apply to a specific toggle call. Takes precedence over configuration from [`model.configureSectionBoxDisplay`](#configure-section-box-display) |

##### `ToggleSectionBoxDisplayOptions`

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| `overrideSectionBoxSetup` | false | false | `boolean` | Determines if `toggleSectionBoxDisplay` should use existing applied section box or if it should set it via its own logic. |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Configure Section Box Display

<p class="heading-link-container"><a class="heading-link" href="#configure-section-box-display"></a></p>

```js
configureSectionBoxDisplay(config);
```

#### Description

Configures how the section box display (controlled by [`model.toggleSectionBoxDisplay`](#toggle-section-box-display)) will appear.

When the section box display is enabled, calling `configureSectionBoxDisplay` will take effect immediately. When the section box display is not enabled, the configuration changes will apply the next time the section box display is enabled.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `config` | false | `ConfigureSectionBoxDisplayConfig` | Configuration object to change the behavior of the section box display. If called with no config argument, it will revert to the default config. |

##### `ConfigureSectionBoxDisplayConfig`

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| `dropdown` | false | true | boolean | Determines whether the section tool dropdown appears. Defaults to true. |
| `hidePlanes` | false | false | boolean | Determines whether the planes of the section box display are shown. When they are hidden, the arrows still show. Defaults to false. |
| `overrideSectionBoxSetup` | false | false | boolean | Determines if [`model.toggleSectionBoxDisplay`](#toggle-section-box-display) should use existing applied section box or if it should set it via its own logic. |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Get Section Box Display

<p class="heading-link-container"><a class="heading-link" href="#get-section-box-display"></a></p>

```ts
getSectionBoxDisplay();
```

#### Description

Gets the current section box display status.

#### Parameters

None

##### Returns

```ts
{
  enabled: boolean,
  configuration: ConfigureSectionBoxDisplayConfig,
  bbox: {
    min: { x: number, y: number, z: number },
    max: { x: number, y: number, z: number },
  },
}
```

For `ConfigureSectionBoxDisplayConfig` schema, see [`model.configureSectionBoxDisplay`](#configure-section-box-display).

##### Namespace

Model

---

### Add Section Plane

<p class="heading-link-container"><a class="heading-link" href="#add-section-plane"></a></p>

```js
addSectionPlane(distance, normal);
```

#### Description

Adds a single section plane.

#### Parameters

| Field Name | Required | Type   | Description                                      |
| ---------- | -------- | ------ | ------------------------------------------------ |
| distance   | true     | Number | The signed distance from the origin to the plane |
| normal     | true     | Object | { x: Number, y: Number, z: Number }              |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Remove Section Plane

<p class="heading-link-container"><a class="heading-link" href="#remove-section-plane"></a></p>

```js
removeSectionPlane(uuid);
```

#### Description

Removes a single section plane.

#### Parameters

| Field Name | Required | Type   | Description                                |
| ---------- | -------- | ------ | ------------------------------------------ |
| uuid       | true     | string | Remove a specific section plane with uuid. |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Set Sections Data

<p class="heading-link-container"><a class="heading-link" href="#set-sections-data"></a></p>

```js
setSectionsData(sectionsData, unit, showCoachmark);
```

#### Description

Applies sectioning described by `sectionsData` using `unit` and showing a "Sectioning Applied" coachmark based on `showCoachmark`.

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| `sectionsData` | `true` | | [`SectionDataAutodeskFormat`](#autodesk-section-data) \| [`SectionDataProcoreFormat`](#bcf-clipping-plane) | The sectioning to apply. |
| `unit` | `false` | `'ft'` | `string` | The unit of `sectionsData`. |
| `showCoachmark` | `false` | `true` | `boolean` | Whether to show "Sectioning Applied" coachmark. |

##### Returns

```js
Promise<true>
```

##### Namespace

Model

---

### Clear Sections

<p class="heading-link-container"><a class="heading-link" href="#clear-sections"></a></p>

```js
clearSection();
```

#### Description

Removes all sections, planes and boxes.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Set Webviewer Configuration options

<p class="heading-link-container"><a class="heading-link" href="#set-webviewer-configuration-options"></a></p>

```js
setOptions(options);
```

#### Description

Sets Webviewer configuration options.
See [Options](#options).
Currently can only set `selection`.

#### Parameters

| Field Name | Required | Type   | Description                                                    |
| ---------- | -------- | ------ | -------------------------------------------------------------- |
| options    | true     | Object | The options to set, currently only supports setting selection. |

##### Returns

```js
undefined
```

##### Namespace

Model

---

### Get relative planmap space position based on model space position

<p class="heading-link-container"><a class="heading-link" href="#get-relative-planmap-space-position-based-on-model-space-position"></a></p>

```js
MapToModelSpace(
  point,
  modelA,
  modelB,
  planA,
  planB,
  imgSize,
  rect,
  direction,
  applyOffset
);
```

#### Description

Retrieve the model space point {x, y} relative to the planmap space point Note: applyOffset affected only when event renderReady has been fired.

#### Parameters

| Field Name  | Required | Type    | Description                                                           |
| ----------- | -------- | ------- | --------------------------------------------------------------------- |
| point       | true     | Object  | current model point:{ x: Number, y: Number}                           |
| modelA      | true     | Object  | model start point:{ x: Number, y: Number}                             |
| modelB      | true     | Object  | model end point:{ x: Number, y: Number}                               |
| planA       | true     | Object  | sheet start point:{ x: Number, y: Number}                             |
| planB       | true     | Object  | sheet end point:{ x: Number, y: Number}                               |
| imgSize     | true     | Object  | original image size:{ x: Number, y: Number}                           |
| rect        | true     | Object  | resized image size, can be equal to imageSize:{ x: Number, y: Number} |
| direction   | true     | Object  | current direction on map point to:{ x: Number, y: Number}             |
| applyOffset | false    | Boolean | apply global offset for calculations.                                 |

##### Returns

```js
Promise<{
  x?: number;
  y?: number;
  dir_x?: number;
  dir_y?: number;
}>
```

##### Namespace

Model

---

### Get relative model space position based on planmap space position

<p class="heading-link-container"><a class="heading-link" href="#get-relative-model-space-position-based-on-planmap-space-position"></a></p>

```js
ModelToMapSpace(
  point,
  modelA,
  modelB,
  planA,
  planB,
  imgSize,
  rect,
  direction,
  applyOffset
);
```

#### Description

Retrieve the planmap space point {x, y} relative to the model space point Note: applyOffset affected only when event renderReady has been fired.

#### Parameters

| Field Name  | Required | Type    | Description                                                           |
| ----------- | -------- | ------- | --------------------------------------------------------------------- |
| point       | true     | Object  | current model point:{ x: Number, y: Number}                           |
| modelA      | true     | Object  | model start point:{ x: Number, y: Number}                             |
| modelB      | true     | Object  | model end point:{ x: Number, y: Number}                               |
| planA       | true     | Object  | sheet start point:{ x: Number, y: Number}                             |
| planB       | true     | Object  | sheet end point:{ x: Number, y: Number}                               |
| imgSize     | true     | Object  | original image size:{ x: Number, y: Number}                           |
| rect        | true     | Object  | resized image size, can be equal to imageSize:{ x: Number, y: Number} |
| direction   | true     | Object  | current direction on map point to:{ x: Number, y: Number}             |
| applyOffset | false    | Boolean | apply global offset for calculations.                                 |

##### Returns

```js
Promise<{
  x?: number;
  y?: number;
  dir_x?: number;
  dir_y?: number;
}>
```

##### Namespace

Model

---

<!-- This is to keep links to the old method name working. -->
<div id="get-global-offset"></div>

### Get Global Offset (Do Not Use)

<p class="heading-link-container"><a class="heading-link" href="#get-global-offset-do-not-use"></a></p>

```js
getGlobalOffsetDoNotUse();
```

#### Description

Retrieves the global offset, if there is one. See [migrating to world coordinates](#migrating-to-world-coordinates) for details on avoiding using this method.

#### Parameters

None

##### Returns

```js
{x: Number, y: Number, z: Number}
```

##### Namespace

Model

---

### Get Object

<p class="heading-link-container"><a class="heading-link" href="#get-object"></a></p>

```js
getObject(id, objectTreeName);
```

#### Description

Returns a model object with the given id. Contains data such as bounding box, hidden/selected states, and parent/child relationships.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| id | true | number | The id of the object |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<{
    id: number, 
    bbox: {
      min: number[],
      max: number[]
    }, 
    nodeType: number,
    hidden: boolean,
    selected: boolean,
    partiallySelected: boolean,
    parentId: number,
    children: number[]
}>
```

##### Namespace

Model

---

### Get Objects

<p class="heading-link-container"><a class="heading-link" href="#get-objects"></a></p>

```js
getObjects(objectIds, objectTreeName);
```

#### Description

Returns an array of model objects as defined by each id in the array. Contains data such as bounding box, hidden/selected states, and parent/child relationships.

If an id is not found, it will not be included in the returned array.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids. |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<[
  {
    id: number, 
    bbox: {
      min: number[],
      max: number[]
    }, 
    nodeType: number,
    hidden: boolean,
    selected: boolean,
    partiallySelected: boolean,
    parentId: number,
    children: number[]
  }
]>
```

##### Namespace

Model

---

### Get Root Object

<p class="heading-link-container"><a class="heading-link" href="#get-root-object"></a></p>

```js
getRootObject(objectTreeName);
```

#### Description

Returns a model object which is the root of the model object tree. Contains data such as bounding box, hidden/selected states, and parent/child relationships.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<
{
    id: number, 
    bbox: {
      min: number[],
      max: number[]
    }, 
    nodeType: number,
    hidden: boolean,
    selected: boolean,
    partiallySelected: boolean,
    parentId: number,
    children: number[]
  } 
  | undefined>
```

##### Namespace

Model

---

### Hide Objects

<p class="heading-link-container"><a class="heading-link" href="#hide-objects"></a></p>

```js
hideObjects(objectIds, objectTreeName);
```

#### Description

Hides objects defined by the array of object ids.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids to hide. |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Hide All Objects

<p class="heading-link-container"><a class="heading-link" href="#hide-all-objects"></a></p>

```js
hideAllObjects(objectTreeName);
```

#### Description

Hides all objects.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Unhide All Objects

<p class="heading-link-container"><a class="heading-link" href="#unhide-all-objects"></a></p>

```js
unhideAllObjects();
```

#### Description

Unhides all objects.

#### Parameters

None

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Unhide Objects

<p class="heading-link-container"><a class="heading-link" href="#unhide-objects"></a></p>

```js
unhideObjects(objectIds, objectTreeName);
```

#### Description

Unhides objects defined by the array of object ids.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids to unhide. |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Get Hidden Objects

<p class="heading-link-container"><a class="heading-link" href="#get-hidden-objects"></a></p>

```js
getHiddenObjects();
```

#### Description

Returns an array of object ids that are hidden.

#### Parameters

None

##### Returns

```js
Promise<number[]>
```

##### Namespace

Model

---

### Get Number Of Objects Hidden

<p class="heading-link-container"><a class="heading-link" href="#get-number-of-objects-hidden"></a></p>

```js
getNumObjectsHidden();
```

#### Description

Retrieve the number of objects selected based on the `FirstObject` counting method.

#### Parameters

None

##### Returns

```js
Promise<{ 
  count: number 
}>
```

##### Namespace

Model

---

### Select Objects

<p class="heading-link-container"><a class="heading-link" href="#select-objects"></a></p>

```js
selectObjects(objectIds, objectTreeName);
```

#### Description

Selects objects defined by the array of object ids.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids to select. |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Select All Objects

<p class="heading-link-container"><a class="heading-link" href="#select-all-objects"></a></p>

```js
selectAllObjects(objectTreeName);
```

#### Description

Selects all objects.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Deselect All Objects

<p class="heading-link-container"><a class="heading-link" href="#deselect-all-objects"></a></p>

```js
deselectAllObjects();
```

#### Description

Deselects all objects.

#### Parameters

None

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Deselect Objects

<p class="heading-link-container"><a class="heading-link" href="#deselect-objects"></a></p>

```js
deselectObjects(objectIds, objectTreeName);
```

#### Description

Deselect objects defined by the array of object ids.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | number[] | An array of object ids to deselect. |
| objectTreeName | false | string | Invokes this method on the object tree specified by the name. Defaults to `default`, and when filtered tree is enabled, can be `filtered` |

##### Returns

```js
Promise<boolean>
```

##### Namespace

Model

---

### Get Selected Objects

<p class="heading-link-container"><a class="heading-link" href="#get-selected-objects"></a></p>

```js
getSelectedObjects();
```

#### Description

Returns an array of object ids that are selected.

#### Parameters

None

##### Returns

```js
Promise<number[]>
```

##### Namespace

Model

---

### Get Number Of Objects Selected

<p class="heading-link-container"><a class="heading-link" href="#get-number-of-objects-selected"></a></p>

```js
getNumObjectsSelected(method);
```

#### Description

Retrieve the number of objects selected based on the count method.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| method | false | string |  Method to count, can be the following: `FirstObject`, `Object`, or `Geometry`. Default value is `FirstObject` |

##### Returns

```js
Promise<{ 
  count: number 
}>
```

##### Namespace

Model

---


### Set X Ray Mode

<p class="heading-link-container"><a class="heading-link" href="#set-x-ray-mode"></a></p>

```js
setXRayMode();
```

#### Description

Sets the rendering mode to X Ray. In this mode, all non selected geometry is rendered with a single color and are transparent.

#### Parameters

None

##### Returns

```js
void
```

##### Namespace

Model

---

### Set Normal Mode

<p class="heading-link-container"><a class="heading-link" href="#set-normal-mode"></a></p>

```js
setNormalMode();
```

#### Description

Sets the rendering mode to normal. In this mode, geometry is rendered using the geometries color and opacity values.

#### Parameters

None

##### Returns

```js
void
```

##### Namespace

Model

---

### Set Navigation Mode

<p class="heading-link-container"><a class="heading-link" href="#set-navigation-mode"></a></p>

```js
setNavigationMode(mode);
```

#### Description

Sets the navigation mode. The navigation mode determines how the camera responds to mouse, keyboard, and wheel events.

| `mode` Value | Description |
| - | - |
| 0 | Default |
| 1 | Fly |
| 2 | Orbit |
| 3 | Measure |
| 4 | Strafe |
| 5 | Pan |
| 6 | Survey |

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| mode | true | number | See above description |

##### Returns

```js
void
```

##### Namespace

Model

---

### Set Measurement

<p class="heading-link-container"><a class="heading-link" href="#set-measurement"></a></p>

```js
setMeasurement(enable);
```

#### Description

Enables or disables the measurement tool. Requires the Measurement Tool be enabled upon viewer initialization by adding `MEASUREMENT_SD` to the options tools array. 

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| enable | true | boolean | True to enable the measurement tool. False to disable. |

##### Returns

```js
void
```

##### Namespace

Model

---

### Filter Object Tree

<p class="heading-link-container"><a class="heading-link" href="#filter-object-tree"></a></p>

```js
filterObjectTree(objectIds);
```

#### Description

Constructs a filtered object tree from an array of object id's. The filtered object tree is used to determine which objects are visible in the Model Objects tool. Additionally, when the filtered object tree is enabled, methods that get, select, or hide nodes can operate on this filtered object tree instead of the full object tree.

When a filtered object tree is enabled, the full object tree is kept in synch with changes made on the filtered tree and vice versa. For example, if you select an object in the filtered tree, the full tree will also have that object selected. This is to ensure that the rendering is kept in synch with the Model Objects tool.

Consider the following example:
```js
// For illustrative purposes, the full tree looks like this
// 0
// ├── 1
// │   ├── 2
// │   └── 3
// └── 4
//     ├── 5 // <-- This object is named `Wall`
//     └── 6

const response = await this.viewer.api.filterObjects({
  filterField: 'name',
  filterValue: 'wall',
});
const objects = await response.json();
const objectIds = objects.map((object) => object.id);
await this.viewer.model.filterObjectTree(objectIds);

// After a filter has been applied, a filtered tree might look like this
// 0
// └── 4
//     └── 5

// Both full and filtered trees are available for you to take actions on.
// For example, you can select a node in the filtered tree.
// This will select the node in the full tree as well and display the object as being selected
// in the Model Objects Tool and in the rendering.
await this.viewer.model.selectObjects([5], 'filtered');

// Any method that would take an optional parameter of `objectTreeName` will default to the full tree.
// Returns:
// {
//    id: 0,
//    children: [1, 4],
//    ...
// }
await this.viewer.model.getRootObject(); 

// You can specify the filtered tree by passing in the name of the filtered tree, in this case 'filtered'
// Returns:
// {
//    id: 0,
//    children: [4],
//    ...
// }
await this.viewer.model.getRootObject('filtered'); 
```

The following methods now accept an optional parameter `objectTreeName`. If not supplied, the default object tree is used. This is the same as passing in `default` for the `objectTreeName` parameter. If a filtered object tree is enabled, the `objectTreeName` parameter can be set to `filtered` to invoke the method on the filtered object tree instead of the default object tree.
- [getObjects](#get-objects)
- [getObject](#get-object)
- [getRootObject](#get-root-object)
- [hideObjects](#hide-objects)
- [hideAllObjects](#hide-all-objects)
- [unhideObjects](#unhide-objects)
- [selectObjects](#select-objects)
- [selectAllObjects](#select-all-objects)
- [deselectObjects](#deselect-objects)



#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| objectIds | true | array | Array of object id's to construct a filtered object tree from. |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Clear Object Tree Filter

<p class="heading-link-container"><a class="heading-link" href="#clear-object-tree-filter"></a></p>

```js
clearObjectTreeFilter();
```

#### Description

Clears the filtered object tree. This will cause the Model Objects tool to display all objects in the model.

#### Parameters

None

##### Returns

```js
Promise<void>
```

##### Namespace

Model

### Get Viewpoint

<p class="heading-link-container"><a class="heading-link" href="#get-viewpoint"></a></p>

```js
getViewpoint();
```

#### Description

Gets the current state of the model as a [`Viewpoint`](#viewpoint), which can be used to create a [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints)

Includes the following:

- applied sectioning
- camera type and orientation

Will currently always return empty redlines as the Web Viewer does not yet have a way to author them.

#### Parameters

None

##### Returns

```js
Promise<Viewpoint>
```

##### Namespace

Model

---

### Set Viewpoint

<p class="heading-link-container"><a class="heading-link" href="#set-viewpoint"></a></p>

```js
setViewpoint(viewpoint);
```

#### Description

Sets the current state of the model to match the `viewpoint`, which can be gotten from endpoints that return a [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints)

Includes the following:

- applied sectioning (similar to calling [`model.setSectionsData`](#set-sections-data))
- camera type and orientation (similar to calling [`camera.setBcfCamera`](#set-camera-from-bcf-camera))
- redlines (similar to calling [`markup.setRedlines`](#set-redlines))
- current render mode (similar to calling [`model.setRenderMode`](#set-render-mode))
- visibility of objects (similar to calling [`model.setVisibility`](#set-visibility))

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `viewpoint` | true | [`Viewpoint`](#viewpoint) | Viewpoint to apply to the model. |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Get Visibility

<p class="heading-link-container"><a class="heading-link" href="#get-visibility"></a></p>

```js
getVisibility();
```

#### Description

Gets the current state of visibility of objects as a [`Visibility`](#visibility).

`visibility` is a field on [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints). If you want to get a whole viewpoint use [`model.getViewpoint`](#get-viewpoint).

#### Parameters

None

##### Returns

```js
Promise<Visibility>
```

##### Namespace

Model

---

### Set Visibility

<p class="heading-link-container"><a class="heading-link" href="#set-visibility"></a></p>

```js
setVisibility(visibility);
```

#### Description

Sets the visibility of objects to match the `visibility`.

`visibility` is a field on [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints). If you want to apply a whole viewpoint use [`model.setViewpoint`](#set-viewpoint).

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `visibility` | true | [`Visibility`](#visibility) | Visiblity to apply to the model. |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

### Get Render Mode

<p class="heading-link-container"><a class="heading-link" href="#get-render-mode"></a></p>

```js
getRenderMode();
```

#### Description

Gets the current [`RenderMode`](#render-mode).

`render_mode` is a field on [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints). If you want to get a whole viewpoint use [`model.getViewpoint`](#get-viewpoint).

#### Parameters

None

##### Returns

```js
Promise<RenderMode>
```

##### Namespace

Model

---

### Set Render Mode

<p class="heading-link-container"><a class="heading-link" href="#set-render-mode"></a></p>

```js
setRenderMode(renderMode);
```

#### Description

Sets the render mode to `renderMode`.

`render_mode` is a field on [BIM Viewpoint](https://developers.procore.com/reference/rest/v1/bim-viewpoints). If you want to apply a whole viewpoint use [`model.setViewpoint`](#set-viewpoint).

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| `renderMode` | true | [`RenderMode`](#render-mode) | Render mode to apply to the model. |

##### Returns

```js
Promise<void>
```

##### Namespace

Model

---

## Markup Namespace

<p class="heading-link-container"><a class="heading-link" href="#markup-namespace"></a></p>

### Clear

<p class="heading-link-container"><a class="heading-link" href="#clear"></a></p>

```js
clear();
```

#### Description

Clears all drawn markup, e.g. from a `markup.draw` call.

#### Parameters

None

##### Returns

```js
void
```

##### Namespace

Markup

---

### Draw

<p class="heading-link-container"><a class="heading-link" href="#draw"></a></p>

```js
draw(markupData);
```

#### Description

Draws various types of markup (ellipses, lines, arrows, texts, and rects) on an SVG canvas based on the provided `markupData`

The `markupData` is an object that contains arrays of different types of markup elements, each with their own properties.

Any existing markup will NOT be cleared. Subsequent draw calls will be drawn on top of previous ones. Use [`markup.clear`](#clear) to clear the screen before subsequent `markup.draw` calls if necessary.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| markupData  | true | MarkupData | Data for the shapes and text to be drawn |

##### MarkupData

The `markupData` object contains arrays of different types of markup elements.

| Field Name | Required | Type | Description |
| - | - | - | - |
| ellipses | false | (BoxEllipses \| RadiusEllipses)[] | Array of ellipse objects to be drawn |
| lines | false | Lines[] | Array of lines objects to be drawn |
| arrows | false | Arrows[] | Array of arrow objects to be drawn |
| texts | false | Text[] | Array of text objects to be drawn |
| rects | false | Rect[] | Array of rect objects to be drawn |

Each type of markup element has its own properties:

##### Shared Properties

These properties are shared by all shapes.

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| stroke | true | | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Stroke color of shape |
| strokeWidth | true | | number | Width of stroke line in px |
| zIndex | false | 0 | number | Shapes with a higher zIndex will be drawn on top of other shapes |

##### Box Ellipses

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| min | true | | { x: number, y: number } | Top-left min point of box containing ellipse in px |
| max | true | | { x: number, y: number } | Bottom-right max point of box containing ellipse in px |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Radius Ellipses

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| center | true | | { x: number, y: number } | Center point of ellipse in px |
| radius | true | | { x: number, y: number } | Radii of ellipse in px. For a circle x and y would be equal |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Lines and Arrows

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| start | true | | { x: number, y: number } | Start point of line in px |
| end | true | | { x: number, y: number } | End point of line in px |

##### Texts

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| origin | true | | { x: number, y: number } | Bottom-left corner of where text will begin in px |
| font | false | '21 px tahoma' | [CSSFont](https://developer.mozilla.org/en-US/docs/Web/CSS/font) | CSS font shorthand declaration to apply to the text |

##### Rects

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| x | true | | number | x component of top-left corner of rectangle in px |
| y | true | | number | y component of top-left corner of rectangle in px |
| width | true | | number | Width of rectangle in px |
| height | true | | number | Height of rectangle in px |
| radius | false | { x: 0, y: 0 } | { x: number, y: number } | Corner-radius in px |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Example Usage

```js
markup.draw({
  ellipses: [
    // Unfilled box ellipse
    {
      stroke: 'rgba(255, 0, 0, 0.5)',
      min: { x: 257.47786244734874, y: 220 },
      max: { x: 320, y: 302.52213755265126 },
      strokeWidth: 3,
    },
    // Filled box ellipse
    {
      stroke: 'rgba(255, 0, 255, 1)',
      fill: 'rgba(0, 255, 255, 0.5)',
      min: { x: 257.47786244734874, y: 300 },
      max: { x: 320, y: 362.52213755265126 },
      strokeWidth: 3,
    },
    // Filled radius ellipse
    {
      stroke: 'chartreuse',
      fill: 'dodgerblue',
      radius: { x: 50, y: 50 },
      center: { x: 50, y: 50 },
      strokeWidth: 5,
    },
  ],
  lines: [
    {
      stroke: 'rgba(0, 255, 0, 0.5)',
      start: { x: 320, y: 240 },
      end: { x: 632.6106877632562, y: 302.52213755265126 },
      strokeWidth: 3,
    },
  ],
  arrows: [
    {
      stroke: 'rgba(0, 0, 255, 0.5)',
      start: { x: 320, y: 240 },
      end: { x: 632.6106877632562, y: 177.47786244734877 },
      strokeWidth: 3,
    },
  ],
  texts: [
    {
      text: 'text',
      origin: { x: 632.6106877632562, y: 240 },
      fill: 'rgba(255, 255, 0, 0.5)',
      font: '9px verdana',
    },
  ],
  rects: [
    {
      x: 250,
      y: 300,
      width: 50,
      height: 100,
      radius: { x: 10, y: 10 },
      fill: 'rgba(255, 0, 0, 0.5)',
      stroke: 'rgba(0, 255, 0, 0.5)',
    },
  ],
})
```

##### Returns

```js
void
```

##### Namespace

Markup

---

### Draw Anchored

<p class="heading-link-container"><a class="heading-link" href="#draw-anchored"></a></p>

```js
drawAnchored(anchoredMarkupData);
```

#### Description

Draws various types of markup (ellipses, lines, arrows, texts, and rects) on an SVG canvas based on the provided `anchoredMarkupData`

The `anchoredMarkupData` is an object that contains arrays of different types of markup elements, each with their own properties.

Once `anchoredMarkupData` is drawn via a call to `drawAnchored`, the markup will be redrawn whenever the [`cameraUpdated`](#cameraupdated) event is fired. The anchored markup will continue drawing at its anchored position in the model until [`markup.clear`](#clear) is called. Unless you are updating the anchor positions, there is no reason to call `drawAnchored` multiple times.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| anchoredMarkupData | true | AnchoredMarkupData | Data for the shapes and text to be drawn |

##### AnchoredMarkupData

The `anchoredMarkupData` object contains arrays of different types of markup elements.

| Field Name | Required | Type | Description |
| - | - | - | - |
| ellipses | false | (AnchoredBoxEllipses \| AnchoredRadiusEllipses)[] | Array of ellipse objects to be drawn |
| lines | false | AnchoredLines[] | Array of lines objects to be drawn |
| arrows | false | AnchoredArrows[] | Array of arrow objects to be drawn |
| texts | false | AnchoredText[] | Array of text objects to be drawn |
| rects | false | AnchoredRect[] | Array of rect objects to be drawn |

##### Shared Properties

Anchored shapes share most properties with their unanchored counterparts used in [`markup.draw`](#draw), but replace properties that position the shape in px coordinates with `anchor` or `anchorStart/anchorEnd`.

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| stroke | true | | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Stroke color of shape |
| strokeWidth | true | | number | Width of stroke line in px |
| zIndex | false | 0 | number | Shapes with a higher zIndex will be drawn on top of other shapes. Anchor position has no effect on draw order. |

##### Anchored Box Ellipses

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| anchor | true | | { x: number, y: number, z: number } | Position that shape will anchor to in model coordinates. Ellipses are anchored at their center. |
| anchorOffset | false | { x: 0, y: 0 } | { x: number, y: number } | Offset in px from anchor position that shape will be drawn |
| min | true | | { x: number, y: number } | Min point of box used to determine size of ellipse. Does not affect position. |
| max | true | | { x: number, y: number } | Max point of box used to determine size of ellipse. Does not affect position. |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Anchored Radius Ellipses

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| anchor | true | | { x: number, y: number, z: number } | Position that shape will anchor to in model coordinates. Ellipses are anchored at their center. |
| anchorOffset | false | { x: 0, y: 0 } | { x: number, y: number } | Offset in px from anchor position that shape will be drawn |
| radius | true | | { x: number, y: number } | Radii of ellipse in px. For a circle x and y would be equal |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Anchored Lines and Anchored Arrows

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| anchorStart | true | | { x: number, y: number, z: number } | Position that start of shape will anchor to in model coordinates. |
| anchorEnd | true | | { x: number, y: number, z: number } | Position that end of shape will anchor to in model coordinates. |

##### Anchored Texts

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| anchor | true | | { x: number, y: number, z: number } | Position that shape will anchor to in model coordinates. Texts are anchored at their bottom-left corner. |
| anchorOffset | false | { x: 0, y: 0 } | { x: number, y: number } | Offset in px from anchor position that shape will be drawn |
| font | false | '21 px tahoma' | [CSSFont](https://developer.mozilla.org/en-US/docs/Web/CSS/font) | CSS font shorthand declaration to apply to the text |

##### Anchored Rects

| Property Name | Required | Default | Type | Description |
| - | - | - | - | - |
| anchor | true | | { x: number, y: number, z: number } | Position that shape will anchor to in model coordinates. Rects are anchored at their top-left corner. |
| anchorOffset | false | { x: 0, y: 0 } | { x: number, y: number } | Offset in px from anchor position that shape will be drawn |
| width | true | | number | Width of rectangle in px |
| height | true | | number | Height of rectangle in px |
| radius | false | { x: 0, y: 0 } | { x: number, y: number } | Corner-radius in px |
| fill | false | 'none' | [CSSColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) | Fill color of shape |

##### Example Usage

```js
markup.drawAnchored({
  ellipses: [
    // Unfilled box ellipse with an anchor offset
    {
      stroke: 'rgba(255, 0, 0, 0.5)',
      min: { x: 0, y: 0 },
      max: { x: 50, y: 50 },
      anchor: { x: 91, y: 8, z: 1 },
      strokeWidth: 3,
    },
    // Filled box ellipse with an anchor offset
    {
      stroke: 'rgba(255, 0, 255, 1)',
      fill: 'rgba(0, 255, 255, 0.5)',
      min: { x: 0, y: 0 },
      max: { x: 50, y: 50 },
      anchor: { x: 91, y: 8, z: 1.5 },
      strokeWidth: 3,
    },
    // Filled radius ellipse
    {
      stroke: 'chartreuse',
      fill: 'dodgerblue',
      radius: { x: 25, y: 25 },
      anchor: { x: 91, y: 8, z: 2 },
      strokeWidth: 5,
    },
    // zIndex'd ellipse on bottom
    {
      stroke: 'yellow',
      fill: 'red',
      radius: { x: 25, y: 25 },
      anchor: { x: 91, y: 8, z: 3.5 },
      anchorOffset: { x: -10, y: -10 },
      strokeWidth: 5,
      zIndex: 1,
    },
    // zIndex'd ellipse on top
    {
      stroke: 'magenta',
      fill: 'blue',
      radius: { x: 25, y: 25 },
      anchor: { x: 91, y: 8, z: 3.5 },
      anchorOffset: { x: 10, y: 10 },
      strokeWidth: 5,
      zIndex: 3,
    },
  ],
  lines: [
    {
      stroke: 'rgba(0, 255, 0, 0.5)',
      anchorStart: { x: 90.5, y: 7.5, z: 2 },
      anchorEnd: { x: 91.5, y: 8.5, z: 2 },
      strokeWidth: 3,
    },
  ],
  arrows: [
    {
      stroke: 'rgba(0, 0, 255, 0.5)',
      anchorStart: { x: 90.5, y: 8.5, z: 2 },
      anchorEnd: { x: 91.5, y: 7.5, z: 2 },
      strokeWidth: 3,
    },
  ],
  texts: [
    {
      text: 'text',
      anchor: { x: 91, y: 8, z: 2.5 },
      anchorOffset: { x: 10, y: 10 },
      fill: 'rgba(255, 255, 0, 0.5)',
      font: '9px verdana',
    },
  ],
  rects: [
    {
      anchor: { x: 91, y: 8, z: 3 },
      width: 40,
      height: 25,
      radius: { x: 10, y: 10 },
      fill: 'rgba(255, 0, 0, 0.5)',
      stroke: 'rgba(0, 255, 0, 0.5)',
    },
    // zIndex'd rect in middle
    {
      anchor: { x: 91, y: 8, z: 3.5 },
      width: 40,
      height: 25,
      radius: { x: 10, y: 10 },
      fill: 'green',
      stroke: 'cyan',
      strokeWidth: 5,
      zIndex: 2,
    },
  ],
})
```

##### Returns

```js
Promise<void>
```

##### Namespace

Markup

---

### Set Redlines

<p class="heading-link-container"><a class="heading-link" href="#set-redlines"></a></p>

```js
setRedlines(redlinesData, fov);
```

#### Description

Draws various types of markup (ellipses, lines, arrows, and texts) on an SVG canvas based on the provided `redlinesData` and `fov` (field of view). This method is similar to [`markup.draw`](#draw), but is more specific to drawing red line markup from Navisworks.

The `redlinesData` is an object that contains arrays of different types of markup elements, each with their own properties.

The `fov` is used to calculate the range of the x and y axes. The tangent of `fov` is used as a range for positive Y values. This is compared to the aspect ratio of the viewport to get a range for positive X values.

Example:

- `fov` is 45deg, tangent of 45 is 1, so positive Y will range from 0 to 1.
- Viewport aspect ratio (determined from canvas width and height) is 2, so positive X will range from 0 to 2.
- For a markup line that starts at (0.5, 1), the line will start exactly in the middle of the first quadrant (upper right).

When called, any existing markup will first be cleared before the markup data is drawn.

After markup drawing is complete, the [`markupDisplayed`](#markupdisplayed) event will be published.

#### Parameters

| Field Name  | Required | Type   | Description                                     |
| ----------- | -------- | ------ | ----------------------------------------------- |
| redlinesData  | true     | object | Data for the shapes and text to be drawn        |
| fov         | true     | number | Field of view to calculate the range of axes    |

#### RedlinesData Object

The `redlinesData` object contains arrays of different types of markup elements.

| Field Name | Required | Type       | Description                          |
| ---------- | -------- | ---------- | ------------------------------------ |
| ellipses   | false    | Ellipses[] | Array of ellipse objects to be drawn |
| lines      | false    | Lines[]    | Array of lines objects to be drawn   |
| arrows     | false    | Arrows[]   | Array of arrow objects to be drawn   |
| text       | false    | Text[]     | Array of text objects to be drawn    |

Each type of markup element has its own properties:

##### Ellipses

| Property Name | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| min_point     | object | Object with `x` and `y` properties              |
| max_point     | object | Object with `x` and `y` properties              |
| color         | object | Object with `r`, `g`, and `b` properties        |
| thickness     | number | Thickness of the ellipse                        |

##### Lines

| Property Name | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| start_point   | object | Object with `x` and `y` properties              |
| end_point     | object | Object with `x` and `y` properties              |
| color         | object | Object with `r`, `g`, and `b` properties        |
| thickness     | number | Thickness of the line                           |

##### Arrows

| Property Name | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| start_point   | object | Object with `x` and `y` properties              |
| end_point     | object | Object with `x` and `y` properties              |
| color         | object | Object with `r`, `g`, and `b` properties        |
| thickness     | number | Thickness of the arrow                          |

##### Texts

| Property Name | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| origin        | object | Object with `x` and `y` properties              |
| color         | object | Object with `r`, `g`, and `b` properties        |
| text          | string | Text content                                    |

##### Returns

```js
void
```

##### Namespace

Markup

---

## Cache Namespace

<p class="heading-link-container"><a class="heading-link" href="#cache-namespace"></a></p>

### Is the model cached

<p class="heading-link-container"><a class="heading-link" href="#is-the-model-cached"></a></p>

```js
hasModel(urlsObject);
```

#### Description

Checks if the internal caching system contains model data for an array of urls.

#### Parameters

| Field Name | Required | Type   | Description                     |
| ---------- | -------- | ------ | ------------------------------- |
| urlsObject | true     | Object | See [Urls Object](#urls-object) |

#### Example

```js
ProcoreBim.Cache.hasModel({
  meshUrl: 'samples/vortex.mesh',
  meshnodeUrl: 'samples/vortex.meshnode',
  nodeUrl: 'samples/vortex.node',
}).then(function (isCached) {
  console.log(isCached);
});
```

##### Returns

```js
Promise(boolean)
```

---

### Remove Model

<p class="heading-link-container"><a class="heading-link" href="#remove-model"></a></p>

```js
removeModel(urlsObject);
```

#### Description

Remove model data for an array of urls.

#### Parameters

| Field Name | Required | Type   | Description                     |
| ---------- | -------- | ------ | ------------------------------- |
| urlsObject | true     | Object | See [Urls Object](#urls-object) |

#### Example

```js
ProcoreBim.Cache.removeModel({
  meshUrl: 'samples/vortex.mesh',
  meshnodeUrl: 'samples/vortex.meshnode',
  nodeUrl: 'samples/vortex.node',
}).then(function (modelRemoved) {
  console.log(modelRemoved);
});
```

##### Returns

```js
Promise(boolean)
```

## GUI Namespace

<p class="heading-link-container"><a class="heading-link" href="#gui-namespace"></a></p>

### Add Context Menu Item

<p class="heading-link-container"><a class="heading-link" href="#add-context-menu-item"></a></p>

```js
addContextMenuItem({label, id, shortcut, onClick, singleObject});
```

#### Description

Retrieves a JavaScript object that describes the object.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| label | true | String | Human-readable label of the new context menu entry |
| id | true | String  | Unique identifier for the new context menu entry |
| shortcut  | true | char[]  | The keyboard-shortcut label to show. Does not register a keyboard listener |
| onClick | true | callback | JS function triggered when menu entry is clicked |
| singleObject | true | boolean | Indicates if the action applies only to single selections |

##### Returns

```js
HTMLDivElement
```

##### Namespace

GUI

---

### Remove Context Menu Items

<p class="heading-link-container"><a class="heading-link" href="#remove-context-menu-items"></a></p>

```js
removeContextMenuItems({contextMenuItemIds});
```

#### Description

Retrieves a JavaScript object that describes the object.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| contextMenuItemIds | true | String[] | Array of the Context Menu Item Ids to remove |

##### Returns

```js
void
```

##### Namespace

GUI

---

### Show Coachmark

<p class="heading-link-container"><a class="heading-link" href="#show-coachmark"></a></p>

```js
showCoachmark({
  id,
  label,
  buttonLabel,
  buttonOnClick,
  tooltipMessage,
});
```

#### Description

Shows a custom coachmark in the Web Viewer.

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| id | true | | String | id of the coachmark that can be used to hide this same coachmark with [`hideCoachmark`](#hide-coachmark). Will also be used to set the id on the coachmark DOM element. |
| label | true | | String | Text that appears in the coachmark. |
| buttonLabel | true | | String | Text that appears in the coachmark button. |
| buttonOnClick | true | | Function | Callback to be called when coachmark button is clicked. |
| tooltipMessage | false | | String | Text to display in a tooltip for the coachmark. If not provided then the coachmark will not have a tooltip. |

##### Returns

```js
void
```

##### Namespace

GUI

---

### Hide Coachmark

<p class="heading-link-container"><a class="heading-link" href="#hide-coachmark"></a></p>

```js
hideCoachmark(id);
```

#### Description

Hides a custom coachmark in the Web Viewer.

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| id | true | | String | id of the coachmark to hide. Must match the id that was passed to [`showCoachmark`](#show-coachmark). |

##### Returns

```js
void
```

##### Namespace

GUI

---

### Set Toolbar Hidden

<p class="heading-link-container"><a class="heading-link" href="#set-toolbar-hidden"></a></p>

```js
setToolbarHidden(hidden);
```

#### Description

Allows you to set the visibility of the bottom toolbar.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| hidden | true | boolean | If true, the toolbar will be hidden. If false, the toolbar will be visible. Throws error if the toolbar is not enabled. |

##### Returns

```js
void
```

##### Namespace

GUI

---

### Set Hotkeys Enabled

<p class="heading-link-container"><a class="heading-link" href="#set-hotkeys-enabled"></a></p>

```js
setHotkeysEnabled(enabled);
```

#### Description

Allows you to enable or disable hotkeys. For example, pressing "X" will turn on the X-Ray mode when hotkeys are enabled. When disabled, the hotkeys will not work and pressing "X" will have no effect.

#### Parameters

| Field Name | Required | Type | Description |
| - | - | - | - |
| enabled | true | boolean | If true, the hotkeys will be enabled. If false, the hotkeys will be disabled.|

##### Returns

```js
void
```

##### Namespace

GUI

---

### Get Equivalent Unit for Display Unit

<p class="heading-link-container"><a class="heading-link" href="#get-equivalent-unit-for-display-unit"></a></p>

```js
getEquivalentUnitForDisplayUnit(unit, displayUnit);
```

#### Description

Returns a unit that matches the dimension of the provided unit in the display unit.

```js
const equivalentLengthUnit = gui.getEquivalentUnitForDisplayUnit('ft', 'm');

console.log(equivalentLengthUnit); // => 'm'

const equivalentAreaUnit = gui.getEquivalentUnitForDisplayUnit('yd²', 'm')

console.log(equivalentAreaUnit); // => 'm²'
```

You might use this if you want to display a value in the display unit set by the user. For example:

```js
const { UOM } = ProcoreBim;
const oneSquareFoot = 1;
const { displayUnits } = viewer.gui.getSettings(); // For example, say it's set to 'm'

console.log(displayUnits); // => 'm'

const equivalentUnit = viewer.gui.getEquivalentUnitForDisplayUnit(
  UOM.sqft,
  displayUnits
);

console.log(equivalentUnit); // => 'm²'

const convertedValue = viewer.gui.convertUnit(
  oneSquareFoot,
  UOM.sqft,
  equivalentUnit
);

console.log(convertedValue); // => 0.09290304

const formattedValue = viewer.gui.formatUnit(convertedValue, equivalentUnit);

console.log(formattedValue); // => '0.0929 m²'
```

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| unit | true | | [Uom](#uom) | The unit you need an equivalent unit for. |
| displayUnit | true | | [DisplayUnit](#display-unit) | The display unit you're trying to represent the unit in. |

##### Returns

```js
Uom
```

##### Namespace

GUI

---

### Convert Unit

<p class="heading-link-container"><a class="heading-link" href="#convert-unit"></a></p>

```js
convertUnit(value, fromUnit, toUnit);
```

#### Description

Converts the value from one unit to another.

```ts
const oneFtInM = gui.convertUnit(1, 'ft', 'm');

console.log(oneFtInM); // => 0.3048
```

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| value | true | | number | The value to convert. |
| fromUnit | true | | [Uom](#uom) | The unit of the value to convert from. |
| toUnit | true | | [Uom](#uom) | The unit to convert to. |

##### Returns

```js
number
```

##### Namespace

GUI

---

### Format Unit

<p class="heading-link-container"><a class="heading-link" href="#format-unit"></a></p>

```js
formatUnit(value, unit);
```

#### Description

Formats the value with the unit.

- Uses locale passed in to [`Webviewer` options](#options) to display correct thousands and decimals separators for the locale.
- Rounds values to 4 decimal places and truncates trailing zeroes.

```ts
const viewerDeLocale = new Webviewer({ locale: 'de-DE', /* ... */ });
const formattedDeLocale = viewerDeLocale.gui.formatUnit(1000.123456789, 'm');

console.log(formattedDeLocale); // => '1.000,1235 m'

const viewerDefaultLocale = new Webviewer({ locale: undefined, /* ... */ });
const formattedDefaultLocale = viewerDefaultLocale.gui.formatUnit(1000.123456789, 'm');

console.log(formattedDefaultLocale); // => '1,000.1235 m'
```

#### Parameters

| Field Name | Required | Default | Type | Description |
| - | - | - | - | - |
| value | true | | number | The value to include in the formatted ouptput. |
| unit | true | | [Uom](#uom) | The unit to include in the formatted output. |

##### Returns

```js
number
```

##### Namespace

GUI

---

### Get Settings

<p class="heading-link-container"><a class="heading-link" href="#get-settings"></a></p>

```js
getSettings();
```

#### Description

Returns user settings.

#### Parameters

None

##### Returns

```js
{
  displayUnits: DisplayUnit
}
```

##### Namespace

GUI

---

## Options

<p class="heading-link-container"><a class="heading-link" href="#options"></a></p>

### Required Options

<p class="heading-link-container"><a class="heading-link" href="#required-options"></a></p>

`parentElement`

##### DOMElement

DOM node to attach viewer to.

```js
{
  parentElement: document.getElementById('myViewer');
}
```

---

`projectId`

##### number

The id of the project the model has been published to.

```js
{
  projectId: 25000;
}
```

---

`companyId`

##### number

The id of the company the model has been published to.

```js
{
  companyId: 10;
}
```

### Required Options for Model Revision

<p class="heading-link-container"><a class="heading-link" href="#required-options-for-model-revision"></a></p>

If you are planning to use the Web Viewer SDK to view a model revision, you will need to provide the following options. These are models published to the Models Tool.

`modelId`

##### number

ID of the associated BIM Model

```js
{
  modelId: 999;
}
```

---

`modelRevisionId`

##### number

ID of the associated BIM Model Revision

```js
{
  modelRevisionId: 888;
}
```

---

`meshUrl`

##### string

URL to mesh binary provided by a Procore service.

```js
{
  meshUrl: 'https://foo.com/geometry/mesh';
}
```

---

`meshnodeUrl`

##### string

URL to mesh binary provided by a Procore service.

```js
{
  meshnodeUrl: 'https://foo.com/geometry/meshnode';
}
```

`nodeUrl`

##### string

URL to mesh binary provided by a Procore service.

```js
{
  nodeUrl: 'https://foo.com/geometry/node';
}
```

### Required Options for File Extraction

<p class="heading-link-container"><a class="heading-link" href="#required-options-for-file-extraction"></a></p>

If you are planning to use the Web Viewer SDK to view a model with a file extraction, you will need to provide the following options. For example, models uploaded to the Documents Tool can be viewed with a file extraction.

`fileExtractionId`

##### number

ID of the file extraction associated to the model.

```js
{
  fileExtractionId: 23;
}
```

### Initializing with Resources

<p class="heading-link-container"><a class="heading-link" href="#initializing-with-resources"></a></p>

The Web Viewer can also be initialized with resource descriptors, which can take the form of either the [required options for model revision](#required-options-for-model-revision) or the [required options for file extraction](#required-options-for-file-extraction).

Support is coming for passing multiple resources in to be able load multiple models.

```js
// File Extraction Initialization 
{
  resources: [
    { fileExtractionId: 23 }
  ],
}

// Model Revision Initialization
{
  resources: [
    {
      modelId: 888,
      modelRevisionId: 999,
      meshUrl: 'https://foo.com/geometry/mesh' 
      meshnodeUrl: 'https://foo.com/geometry/meshnode' 
      nodeUrl: 'https://foo.com/geometry/node' 
    }
  ],
}
```

### Optional Options

<p class="heading-link-container"><a class="heading-link" href="#optional-options"></a></p>

`accessToken`

##### string

OAuth access token used to authenticate requests to the Procore API made by the Web Viewer SDK on your behalf (e.g. fetching an object's properties). Specifically it adds an `Authorization` header with the provided token to each request. See [Making Your First API Call](/documentation/making-first-call) for details on retrieving an access token.

Note also that this setup implies an Implicit Grant flow in that your access token will be present on the browser rather than only present on a server, which may be a security concern. See [OAuth 2.0 Implicit Grant](/documentation/oauth-implicit-flow) for details on Implicit Grant flow.

If this is not provided as an option, you will need to intercept the SDK's requests and attach the `Authorization` header yourself in order for them to succeed.

Additionally, the access token will eventually expire (in 24 hours currently for an implicitly granted token) and requests made by the SDK will fail. We don't yet have a way to notify of these errors and refresh the in-use access token. One workaround is to keep track of the expiration time for the access token (provided in the `expires_in` query param in the Implicit Grant flow) and re-instantiate the `ProcoreBim.Webviewer` with the new access token.

```js
{
  accessToken: '1234567890abdefghijkl',
}
```

---

`baseUrl`

##### string

The `baseUrl` is a mechanism for determining which version of the Procore API you will hit.

For example, while developing a third-party Procore App, you may want to utilize one of the [Sandbox Environments](https://developers.procore.com/documentation/development-environments). To use the Development Sandbox, you could pass `baseUrl: 'https://sandbox.procore.com/'` and any requests made by the SDK will be made to that origin.

Another example is if you want to avoid using the Implicit Grant flow you might set up a server at say `https://auth-proxy.myapp.com` and pass that in as the `baseUrl`. All SDK requests will then be made to your server which could add the `Authorization` header and make the actual request to the Procore API.

```js
{
  baseUrl: 'https://api.procore.com',
}
```

---

`bcfCamera`

##### Object

Initializes the Webviewer with the camera.

```js
{
  bcfCamera: {
    perspective_camera: {
      camera_view_point: {
        x:100,
        y:100,
        z:10
      },
      camera_direction: {
        x:0,
        y:1,
        z:0
      },
      camera_up_vector: {
        x:0,
        y:0,
        z:1
      },
      field_of_view: 1.3,
      target_distance: 1000000
    }
  }
}
```

--- 

`min_boundary`

##### Object

Defines the minimum boundary of the model.
If `max_boundary` is also defined, both `min_boundary` and `max_boundary` are used to create a section box which will only render objects within that section box.

```js
{
  min_boundary: {
      z:"-14.1686084710",
      y:"20.1924446284",
      x:"-26.6485348162"
  }
}

```

---

`max_boundary`

##### Object

Defines the maximum boundary of the model.
If `min_boundary` is also defined, both `max_boundary` and `min_boundary` are used to create a section box which will only render objects within that section box.

```js
{
  max_boundary: {
      z:"11.8769418043",
      y:"155.7780451539",
      x:"17.0583084307"
  }
}
```

---

`rotation`

##### Object

If `min_boundary` and `max_boundary` are defined, then you can also set the rotation of the section box.

```js
{
  rotation: {
      z:"11.8769418043",
      y:"155.7780451539",
      x:"17.0583084307"
  }
}
```

---

`multiselect`

##### boolean

Sets the object selection mode.

```js
{
  multiselect: false;
}
```

---

`tools`

##### Array

```js
[
  ProcoreBim.Webviewer.tools.BOTTOMTOOL,
  ProcoreBim.Webviewer.tools.COACHMARKS,
  ProcoreBim.Webviewer.tools.MEASUREMENT_SD
];
```

Pre-built tools to be enabled.

This array can be composed of both constants and objects.
You can use objects to set tool specific options.
When an object is passed, the object MUST have the key, type and the value be the tool name.
Other keys in this object are options you want to set.

For example:

```js
[
  ProcoreBim.Webviewer.tools.BOTTOMTOOL,
  { type: ProcoreBim.Webviewer.tools.MEASUREMENT_SD, center_lock: true }
];
```

See [Tools](#tools) for further information.

---

`locale`

##### string

```js
{
  locale: 'en-US'
}
```

Locale to use for display language, number formatting, and other localization.

Defaults to `'en'`.

## Objects

<p class="heading-link-container"><a class="heading-link" href="#objects"></a></p>

### Perspective Camera Object

<p class="heading-link-container"><a class="heading-link" href="#perspective-camera-object"></a></p>

```js
{
  perspective_camera: {
    camera_view_point:  {x: Number, y: Number, z: Number},
    camera_direction:  {x: Number, y: Number, z: Number},
    camera_up_vector:  {x: Number, y: Number, z: Number},
    field_of_view: Number,
    target_distance: Number
  }
}
```

### Orthogonal Camera Object

<p class="heading-link-container"><a class="heading-link" href="#orthogonal-camera-object"></a></p>

```js
{
  orthogonal_camera: {
    camera_view_point: {x: Number, y: Number, z: Number},
    camera_direction: {x: Number, y: Number, z: Number},
    camera_up_vector: {x: Number, y: Number, z: Number},
    view_to_world_scale: Number,
    target_distance: Number,
    vertical_extent: Number
  }
}
```

### Meshnode

<p class="heading-link-container">
  <a class="heading-link" href="#meshnode"></a>
</p>

```js
{
  bbox: Float32Array(6),
  matrix: Float32Array(12),
  meshData: [
    {
      byteLength: Number,
      color: Number,
      faces: Uint16Array(n),
      offset: Number,
      opacity: Number,
      vertices: Float32Array(n)
    }
  ]
}
```

### BCF Clipping Plane

<p class="heading-link-container">
  <a class="heading-link" href="#bcf-clipping-plane"></a>
</p>

The BCF Clipping Plane is based off of the BCF schema defined here: https://github.com/buildingSMART/BCF-API#3526-clipping-plane

The one addition we've made is the `unit` field. If the `unit` is not present, we will assume it to be `'ft'`.

```ts
{
  unit?: string; // Assumed to be "ft" if not present.
  direction: {
    x: number;
    y: number;
    z: number;
  };
  location: {
    x: number;
    y: number;
    z: number;
  }
}
```

### Autodesk Section Data

<p class="heading-link-container">
  <a class="heading-link" href="#autodesk-section-data"></a>
</p>

```ts
{
  Type: 'ClipPlaneSet';
  Version: 1;
  Unit?: string;
  OrientedBox?: {
    Type: 'OrientedBox3D';
    Version: 1;
    Box: [
      [number, number, number], // [minX, minY, minZ]
      [number, number, number]  // [maxX, maxY, maxZ]
    ];
    Rotation: [number, number, number]; // [degX, degY, degZ]
  } | null;
  Planes?: {
    Type: 'ClipPlane';
    Version: 1;
    Normal: [number, number, number]; // [dirX, dirY, dirZ]
    Distance: number;
    Enabled: boolean;
  }[] | null;
  Linked: boolean;
  Enabled: boolean;
};
```

### Viewpoint

<p class="heading-link-container">
  <a class="heading-link" href="#viewpoint"></a>
</p>

```ts
{
  camera_data: string; // JSON string
  sections_data: string; // JSON string
  redlines_data: string; // JSON string
  visibility: Visibility;
  render_mode: RenderMode;
};
```

### Visibility

<p class="heading-link-container">
  <a class="heading-link" href="#visibility"></a>
</p>

```ts
{
  default_visibility: boolean; 
  exceptions: {
    object_ids: number[];
    object_ranges: [number, number][];
  };
};
```

This object is effectively representing a flat list of object ids that are hidden, but uses some strategies to reduce the JSON size of this list.

`default_visibility` determines whether all objects are hidden or visible.

`exceptions` determines which objects are not in the `default_visibility` state.

`exceptions.object_ids` is a flat list of object ids.

`exceptions.object_ranges` is a list of range objects where the first number is the object id at the start of the range and the second number is the number of items in the range.

#### Examples

#### Some Hidden with a Range

```js
{
  default_visibility: false,
  exceptions: {
    object_ids: [],
    object_ranges: [
      [1, 3], // Expands to [1, 2, 3]
      [1000, 2] // Expands to [1000, 1001]
    ]
  }
}
```

##### All Hidden

```js
{
  default_visibility: false,
  exceptions: {
    object_ids: [],
    object_ranges: []
  }
}
```

Because `default_visibility` is `false`, all objects are hidden by default and because there are no `exceptions` we end with all objects hidden.

##### Less than Half Hidden

```js
{
  default_visibility: true,
  exceptions: { // Represents objects that are hidden
    object_ids: [ 7, 10, 13, 20, /* ... many more */ ]
    object_ranges: [ [1, 3], [1000, 2], /* ... many more */ ]
  }
}
```

When less than ~half of objects are hidden, `default_visibility` is set to `true` and `exceptions` represents objects that are hidden.

##### More than Half Hidden

```js
{
  default_visibility: false,
  exceptions: { // Represents objects that are visible
    object_ids: [ 8, 11, 14, 21, /* ... many more */]
    object_ranges: [ [4, 3], [1003, 2], /* ... many more */ ]
  }
}
```

When more than ~half of objects are hidden, `default_visibility` is set to `false` and `exceptions` represents objects that are visible.

#### Code to Serialize/Deserialize Ranges

We do not intend for external parties to _need_ to parse the ranges, but if you find yourself needing to here are some TS functions for converting from a flat list to a list of ranges and vice versa.

```ts
// Takes an array of numbers and extracts contiguous numbers into ranges.
// Non-contiguous numbers are returned as ids. Each range is a tuple of
// [startId, numberOfIdsInRange].
// So [1, 2, 3, 5, 6, 7] would become [[1, 3], [5, 3]]
export const rangify = (
  numbers: number[]
): { numbers: number[]; ranges: [number, number][] } => {
  const sortedA = Array.from(numbers);
  sortedA.sort((a, b) => a - b);
  let numInRange = 0;
  return sortedA.reduce<{ numbers: number[]; ranges: [number, number][] }>(
    (acc, curr, i, orig) => {
      if (numInRange > 0) {
        numInRange -= 1;
        return acc;
      }
      const next = orig[i + 1];
      const delta = next - curr;
      if (delta !== 1) {
        acc.numbers.push(curr);
      } else {
        let j = i + 1;
        while (j < orig.length - 1 && orig[j + 1] - orig[j] === 1) {
          j += 1;
        }
        numInRange = j - i;
        const range: [number, number] = [curr, numInRange + 1];
        acc.ranges.push(range);
      }
      return acc;
    },
    {
      numbers: [],
      ranges: [],
    }
  );
};

// Takes a rangified object and returns an unsorted array of numbers.
export const derangify = (rangified: {
  numbers: number[];
  ranges: [number, number][];
}): number[] => {
  const hiddenArray = [...rangified.numbers];
  rangified.ranges.forEach((range) => {
    const expandedRange = Array.from(
      { length: range[1] },
      (_, index) => range[0] + index
    );
    hiddenArray.push(...expandedRange);
  });
  return hiddenArray;
};
```

### Render Mode

<p class="heading-link-container">
  <a class="heading-link" href="#render-mode"></a>
</p>

```ts
'shaded' | 'xray'
```

### Urls Object

<p class="heading-link-container">
  <a class="heading-link" href="#urls-object"></a>
</p>

```js
{
  meshUrl: String,
  meshnodeUrl: String,
  nodeUrl: String,
}
```

## Constants

<p class="heading-link-container">
  <a class="heading-link" href="#constants"></a>
</p>

### Uom

<p class="heading-link-container">
  <a class="heading-link" href="#uom"></a>
</p>

The `UOM` constant holds all valid unit of measure strings. The `Uom` type is a union of all these.

```ts
const candelasPerSquareMeter: Uom = UOM.cdsqm;

console.log(candelasPerSquareMeter); // => "cd/m²"
```

### Display Unit

<p class="heading-link-container">
  <a class="heading-link" href="#display-unit"></a>
</p>

Display units are the unit to display values in. It can be changed by the user in the Settings window. It can also be queried via [`gui.getSettings`](#get-settings).

The `DISPLAY_UNIT` constant holds all possible display units. The `DisplayUnit` type is a union of all these.

```ts
const feetInches: DisplayUnit = DISPLAY_UNIT.FeetInches

console.log(feetInches); // => "ftin"
```

### Tools

<p class="heading-link-container">
  <a class="heading-link" href="#tools"></a>
</p>

`ProcoreBim.Webviewer.tools.{tool_type}`

| Tool             | Description                                                 |
| ---------------- | ----------------------------------------------------------- |
| BOTTOMTOOL       | Adds the bottom toolbar that will house other enabled tools |
| COACHMARKS       | All coachmarks                                              |
| COACHMARKSECTION | Section Applied coachmark label                             |
| COACHMARKSHIDDEN | Hidden Objects coachmark label                              |
| CONTEXTMENU      | Right click context menu                                    |
| FLOORPLAN        | Adds 2D Navigation minimap and modal                        |
| LOADING          | Adds loading screens for viewer initialization              |
| MEASUREMENT_SD   | Shortest Distance tool                                      |
| MODELVIEWS       | Views Window                                                |
| SETTINGS         | Settings Window to change unit display                      |
| VIEW_PROPERTIES  | Properties Window to display properties of an object        |
| OBJECTMODELTREE  | Object Tree Window                                          |
| XRAY_MODE        | Adds button to toggle Xray Mode                             |
| SECTION_BOX      | Adds button to toggle interactive Section Box               |

`MEASUREMENT_SD`

| Option      | Type    | Description                                                                                                                                         |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| center_lock | Boolean | Enables or disables the measurement navigation mode. Set to false if you don't want the navigation to change in anyway after a measurement is made. |

<!-- markdownlint-enable no-inline-html -->

## Migration Guides

<p class="heading-link-container">
  <a class="heading-link" href="#migration-guides"></a>
</p>

### v16 to v17

<p class="heading-link-container">
  <a class="heading-link" href="#v16-to-v17"></a>
</p>

#### Change `models.getGlobalOffset` to `models.getGlobalOffsetDoNotUse`

[`models.getGlobalOffset`](#get-global-offset) has been renamed to [`models.getGlobalOffsetDoNotUse`](#get-global-offset-do-not-use) and its return type has been modified:

```ts
// v16
await models.getGlobalOffset()
// => { offsetX: 0.1, offsetY: 0.2, offsetZ: 0.3 }

// v17
await models.getGlobalOffsetDoNotUse()
// => { x: 0.1, y: 0.2, z: 0.3 }
```

See [migrating to world coordinates](#migrating-to-world-coordinates) for context and on how to avoid needing this method.

### v15 to v16

<p class="heading-link-container">
  <a class="heading-link" href="#v15-to-v16"></a>
</p>

#### Removal of `options.featureFlags.enableObjectTreeSearch`

Object tree search is now always enabled and passing this option when instantiating `Webviewer` will be ignored.

### v14 to v15

<p class="heading-link-container">
  <a class="heading-link" href="#v14-to-v15"></a>
</p>

#### Convert `camera.getSnapshot` to Use Named Parameters

[`model.getSnapshot`](#get-snapshot) now expects an optional params object instead of individual params. This aligns the signature to [`model.getSnapshotDataUrl`](#get-snapshot-data-url) which already takes an optional params object with the same options.

It previously took one optional argument that set the background color of the snapshot. If you were using this functionality you will need to update it as follows:

```ts
// OLD
model.getSnapshot('#aabbcc');

// NEW
model.getSnapshot({ color: '#aabbcc' });
```

If you were calling `model.getSnapshot()` without arguments, no change is required.

### v13 to v14

<p class="heading-link-container">
  <a class="heading-link" href="#v13-to-v14"></a>
</p>

#### TypeScript Type Declarations

The published package now includes TypeScript type declarations. If you are using TypeScript, you will now get type checking and autocompletion for the SDK.

You may expierence some issues with the new type declarations if some code uses a type incorrectly. These will appear in the form of TypeScript errors at compile time and if not resolved, your application may not run correctly.

### v12 to v13

<p class="heading-link-container">
  <a class="heading-link" href="#v12-to-v13"></a>
</p>

The options object passed to the `ProcoreBim.Webviewer` constructor will now require a `projectId` and `companyId` field. `projectId` is used for many requests to the Procore API. `companyId` is used to set the `Procore-Company-Id` header to comply with requirements for Multiple Procore Regions, see https://developers.procore.com/notifications/67 for more info. If you were not passing these fields before, you will need to add them to your options object.

```ts
{
  projectId: number;
  companyId: number;
}

```

### v11 to v12

<p class="heading-link-container">
  <a class="heading-link" href="#v11-to-v12"></a>
</p>

#### Change in Default Behavior of `model.toggleSectionBoxDisplay`

The default behavior of [`model.toggleSectionBoxDisplay`](#toggle-section-box-display) has changed to set up the section box following the same logic as the "Section Box" toolbar button. This was to make implementing section box display easier by default while still allowing for customization if it's desired.

Previously, calling [`model.toggleSectionBoxDisplay`](#toggle-section-box-display) would throw an error if called when there was no section box set (e.g. via [`model.setSectionBox`](#set-section-box)). So if you were using it before you were likely doing something like this:

```ts
model.setSectionBox(
  { x: -1, y: -1, z: -1 },
  { x: 1, y: 1, z: 1 },
  { x: 0, y: 0, z: 0 },
  false,
);
model.toggleSectionBoxDisplay(true)
```

This code will still run, but [`model.toggleSectionBoxDisplay`](#toggle-section-box-display) now will internally set the section box based on its own logic so the [`model.setSectionBox`](#set-section-box) will not do anything.

If you'd like to stay in control of setting the section box then you can override this behavior globally using [`model.configureSectionBoxDisplay`](#configure-section-box-display)

```ts
// Calling this will tell all subsequent calls to model.toggleSectionBoxDisplay 
// NOT to set the section box. It will result in the old behavior where it throws
// an error if no section box is set.
model.configureSectionBoxDisplay({ overrideSectionBoxSetup: true })

model.setSectionBox(
  { x: -1, y: -1, z: -1 },
  { x: 1, y: 1, z: 1 },
  { x: 0, y: 0, z: 0 },
  false,
);
model.toggleSectionBoxDisplay(true)
```

Or you can override the behavior on a per call basis by passing an option argument to [`model.toggleSectionBoxDisplay`](#toggle-section-box-display)

```ts
model.setSectionBox(
  { x: -1, y: -1, z: -1 },
  { x: 1, y: 1, z: 1 },
  { x: 0, y: 0, z: 0 },
  false,
);
// This will tell model.toggleSectionBoxDisplay NOT to set the section box,
// but just for this call. Other calls to model.toggleSectionBoxDisplay will 
// be unaffected.
model.toggleSectionBoxDisplay(true, { overrideSectionBoxSetup: true })
```

#### Continuation of consistent world coordinates

These methods now return model objects with `bbox` properties in world coordinates:

- `model.getObject`
- `model.getObjects`
- `model.getRootObject`

These events now return payloads with model objects with `bbox` properties in world coordinates:

- `objectSingleClick`
- `objectSelect`

If you were using these methods or events, you will need to update your code to be in the appropriate coordinate system. See [Migrating to World Coordinates](#migrating-to-world-coordinates)

### v10 to v11

<p class="heading-link-container">
  <a class="heading-link" href="#v10-to-v11"></a>
</p>

The [`projectSettingsUpdated`](#projectsettingsupdated) and [`projectSettingsLoaded`](#projectsettingsloaded) events' payloads' `units` field has changed names to `displayUnits`.

```ts
// Old payload
{ units: string }

// New payload
{ displayUnits: string }
```

### v9 to v10

<p class="heading-link-container">
  <a class="heading-link" href="#v9-to-v10"></a>
</p>

The [`intersectPointClick`](#intersectpointclick) event's payload has changed to return "world coordinates".

If you were using the payload from this event, you will need to update your code to be in the appropriate coordinate system. See [Migrating to World Coordinates](#migrating-to-world-coordinates)

### v8 to v9

<p class="heading-link-container">
  <a class="heading-link" href="#v8-to-v9"></a>
</p>

#### Changes to `camera.getSnapshot` and  `camera.getSnapshotDataUrl` methods

These methods now return a Promise that resolves to a `void` and a `string` respectively. Previously these methods would run synchronously and `camera.getSnapshotDataUrl` would return a `string` directly.

#### Recommended changes you can make

```
// Before
const dataUrl = viewer.camera.getSnapshotDataUrl();

// After
const dataUrl = await viewer.camera.getSnapshotDataUrl();
```

### v7 to v8

<p class="heading-link-container">
  <a class="heading-link" href="#v7-to-v8"></a>
</p>

#### Coordinates Should Be Consistent with the Source File

Several methods now return or expect to receive "world coordinates".

The changed methods:

- `camera.getPosition` now returns a point in world coordinates.
- `camera.setPosition` now expects a point in world coordinates.
- `camera.getLookAt` now returns a point in world coordinates.
- `camera.setLookAt` now expects a point in world coordinates.
  - NOTE: this method has a return value that is still NOT in world coordinates. Prefer using `camera.getPosition/getLookAt/getBcfCamera` if you want the resulting position after a set.
- `camera.getBcfCamera`'s return value's `camera_view_point` is now in world coordinates.
- `camera.setBcfCamera` took a second boolean argument that defaulted to receiving local coordinates. If you were passing `false` or nothing here it will no longer be consistent with the coordinates from other methods. If you were passing `true` then no change required.
- `model.ModelToMapSpace` now expects a `point` parameter in world coordinates.

If you were using these methods, you will need to update your code to be in the appropriate coordinate system. See [Migrating to World Coordinates](#migrating-to-world-coordinates)

#### `model.getSections` Now Requires Format Parameter

Calling `model.getSections()` without argument will now throw an error. You must pass either `"autodesk"` or `"bcf"`.

The previous return shape looked like this:

```ts
  {
    type: "box",
    box:
    {
      min:{ x: Number, y: Number, z: Number},
      max:{ x: Number, y: Number, z: Number},
      rotation: undefined | Number
    }
  } 
| {
  type: "plane",
  plane:
  [
    {
      distance: Number,
      normal: {normalX: Number, normalY: Number, normalZ: Number},
      uuid: String
    }
  ]
}
```

In this format you would get a different shape based on whether section planes or a section box had been set. This format is no longer available so you will need to convert your code to one of the new formats if you were using it.

The `"autodesk"` format is closer to the previous return shape in that it supports both a box return type and a plane return type. For historical reasons, it is also the format that most section data in Procore is stored in. See [Autodesk Section Data object](#autodesk-section-data).

The `"bcf"` format is a bit less esoteric, but will always return an array of planes even if you had set a box. See [BCF clipping plane](#bcf-clipping-plane).

##### Conversion Scripts

If you have stored the data previously returned by `model.getSections()` you may consider converting that data to one of the new formats. For example, if you wanted to create a [Procore Coordination Issue](https://developers.procore.com/reference/rest/v1/coordination-issues?version=1.0#create-coordination-issue) with section data you had been storing, at the time of this writing you would need to convert the old data to the autodesk format.

Alternatively you may need to convert to BCF to interoperate with other systems.

The scripts here are a starting off point to help with such conversions. However, please test them on your own systems and validate that they handle your data.

##### Conversion Script: Old Format to `"autodesk"`

```js
const convertOldSectionDataToAutodesk = (oldSectionData) => {
  if (oldSectionData.type === 'box') {
    const {
      box: { min, max },
    } = oldSectionData;
    let {
      box: { rotation },
    } = oldSectionData;
    if (rotation === null) {
      rotation = { x: 0, y: 0, z: 0 };
    }
    return {
      Type: 'ClipPlaneSet',
      Version: 1,
      Unit: 'ft',
      OrientedBox: {
        Type: 'OrientedBox3d',
        Version: 1,
        Box: [
          [min.x, min.y, min.z],
          [max.x, max.y, max.z],
        ],
        Rotation: [rotation.x, rotation.y, rotation.z],
      },
      Planes: null,
      Enabled: true,
      Linked: false,
    };
  }
  if (oldSectionData.type === 'plane') {
    return {
      Type: 'ClipPlaneSet',
      Version: 1,
      Unit: 'ft',
      OrientedBox: null,
      Planes: oldSectionData.plane.map((plane) => {
        const { normal, distance } = plane;
        return {
          Type: 'ClipPlane',
          Version: 1,
          Normal: [normal.normalX, normal.normalY, normal.normalZ],
          Distance: distance,
          Enabled: true,
        };
      }),
      Enabled: true,
      Linked: false,
    };
  }
  throw new Error(`Unrecognized old section data type: ${oldSectionData.type}`);
};
```

##### Conversion Script: Old Format to `"bcf"`

This one is a little tricky in the case of converting box data to planes. If this is your use case please reach out in your integrator slack channel and we'll find a way to expose this more easily.

```js
const convertOldSectionDataToBcf = (oldSectionData) => {
  if (oldSectionData.type === 'box') {
    throw new Error('Please reach out if you need assistance with this use case. It requires a math library or more code than is reasonable to put here.');
  }
  if (oldSectionData.type === 'plane') {
    return oldSectionData.plane.map((plane) => {
      const { normal, distance } = plane;
      return {
        unit: 'ft',
        direction: {
          x: normal.normalX,
          y: normal.normalY,
          z: normal.normalZ,
        },
        location: {
          x: -normal.normalX * distance + 0, // Add 0 to avoid -0 values.
          y: -normal.normalY * distance + 0,
          z: -normal.normalZ * distance + 0,
        },
      };
    });
  }
  throw new Error(`Unrecognized old section data type: ${oldSectionData.type}`);
};
```

### v6 to v7

<p class="heading-link-container">
  <a class="heading-link" href="#v6-to-v7"></a>
</p>

#### Changes to Payloads of `objectSelect` and `objectSingleClick` Events

##### New Payload

In versions < v7 the `objectSelect` and `objectSingleClick` events had a payload of the meshnode index that was clicked. They now have a new shape that is based around object id instead of meshnode index.

The new shape looks like this:
```ts
{
  objectId: 3,
  selectionContainerId: 2,
  ancestry: [
    { 
      id: 1,
      parentId: undefined,
      children: [
        { id: 2, parentId: 1, children: [3, 42] },
      ],
    },
    { 
      id: 2,
      parentId: 1,
      children: [
        { id: 3, parentId: 2, children: [] },
        { id: 42, parentId: 2, children: [] },
      ] 
    },
    { 
      id: 3,
      parentId: 2,
      children: [],
    },
  ]
}
```

The `objectId` is the object id of the object that was clicked/selected

The `selectionContainerId` is the object id of the object that contains the object that was clicked/selected. In v7 this will always be the "First Object".

The `ancestry` attribute is intended to tell you all the information needed to recreate the subtree for what was clicked/selected. Each item in the array represents a depth of the object tree, starting at the root object and going down to the leaf object that was clicked/selected. The shape of these objects is the same that is returned by `model.getObject` with the one exception that the first level of children are fully materialized objects instead of just being object ids. This is to capture the siblings of each item without an additional request as these would be displayed as closed items in an object tree implementation (object id 42 in the example above.)

See [`objectSelect`](#objectselect) or [`objectSingleClick`](#objectsingleclick) for more details on these payloads.

##### Converting from Old Payload

We encourage building around object ids moving forward, but if you need a straight conversion of the new payload to the previous one you could modify event listeners on `objectSelect` or `objectSingleClick` like so:

```ts
viewer.events.addEventListener('objectSelect', async (payload) => {
  const { objectId } = payload;
  const { meshnodeIndex } = await viewer.model.getMeshnodeFromObjectId(objectId)

  // meshnodeIndex should be the same as the < v7 payload.
})
```

### v5 to v6

<p class="heading-link-container">
  <a class="heading-link" href="#v5-to-v6"></a>
</p>

Methods in the model namespace have been renamed to better match what they return and what type of parameters they expect. Primarily this has meant clarifying that the particular type of "id" that these methods expect is a "meshnode index". To be clear: there is no behavior change of these methods, they have only been renamed and will continue to work as they have been.

#### Method Renames

To migrate you can safely do a find a replace for each of these renamed methods:

```
model.getObject => model.getMeshnode
model.getObjectFromPropertyId => model.getMeshnodeFromObjectId
model.getHiddenGeoIds => model.getHiddenMeshnodeIndices
model.addHiddenGeoIds => model.addHiddenMeshnodeIndices
model.hasHiddenGeoIds => model.hasHiddenMeshnodeIndices
model.clearHiddenIds => model.clearHiddenMeshnodeIndices
model.getSelectedGeoIds => model.getSelectedMeshnodeIndices
model.addSelectedIds => model.addSelectedMeshnodeIndices
model.hasSelectedIds => model.hasSelectedMeshnodeIndices
model.clearSelectedIds => model.clearSelectedMeshnodeIndices
model.setObjectColor => model.setMeshnodeColor
```

Usages of `model.getModelItemBoundary` can be found and replaced with a call to `model.getMeshnode`.

```
model.getModelItemBoundary(meshnodeIndex) => model.getMeshnode(meshnodeIndex).bbox
```

#### Reasoning

These methods referred to `object`, `objectId`, `propertyId`, `geoId`, and simply `id`. There are two issues with this: (1) There are multiple names for the same thing. As it currently stands, most things are actually referring to a `meshnodeIndex` or a `meshnode`. (2) What we referred to as an `object`, e.g. what’s returned from the old `getObject`, is not what the BIM Rest API refers to as an object. We believe these method renames will bring more clarity to how to use them in your own code.

#### Future Plans

We intend to release another set of methods that operate on objects (of which meshnodes are a subtype) and object ids rather than directly on meshnodes and meshnode indices. These object methods will be released in a non-breaking way and the meshnode methods will continue to work. However, the meshnode methods may eventually be deprecated or become considered internal, meaning they are not subject to semver.

### v4 to v5

<p class="heading-link-container">
  <a class="heading-link" href="#v4-to-v5"></a>
</p>

New `camera.zoomTo*` methods were added and the naming of `camera.zoomExtents` (added in v3.1.0) no longer really made sense.

#### Method Renames

To migrate you can safely do a find a replace for each of these renamed methods:

```
camera.zoomExtents => camera.zoomToBoundingBox
```

### v3 to v4

<p class="heading-link-container">
  <a class="heading-link" href="#v3-to-v4"></a>
</p>

There were no actual API changes that necesitated a breaking change here but we did drastically change our rendering algorithm to reduce flashing and dropout. For larger models this may come at the expense of low framerates.

I have the privilege of writing this migration guide from the future and can tell you that we've been able to make it even better without (as much) of a framerate hit for larger models in v6.0.1 and you should consider upgrading to that or later. v3 to v4 may also not have needed a breaking change in retrospect so you can safely go from v3 to v4 without your code breaking but know that rendering will behave and perform differently and hopefully mostly for the better on v4 (but again vastly better on v6).

### Migrating to World Coordinates

<p class="heading-link-container">
  <a class="heading-link" href="#migrating-to-world-coordinates"></a>
</p>

#### Context

Historically, many methods and events returned and/or expected to receive coordinates that were not consistent with the model coordinates from the source file. To get the correct coordinates you would need to add/subtract the result of `model.getGlobalOffsetDoNotUse` to them. This would affect models that are significantly offset from the origin, which we refer to as being in "world coordinates".

We have since taken the stance that all coordinates returned or required as parameters to the Web Viewer should be consistent with the model coordinates from the source file. However, we will be updating them as we come across them so they may be shipped across multiple breaking changes.

#### Note for Versions Before v17

Prior to v17, `model.getGlobalOffsetDoNotUse` was called `model.getGlobalOffset` and had a different return type. See [v16 to v17 migration guide](#v16-to-v17) for more details.

#### Migration Guide

If you were saving data returned from a method or event payload that has changed, that data may now be inconsistent if there is a global offset (i.e. if `model.getGlobalOffsetDoNotUse` is a non-zero vector) for that model. This can result in behavior where setting the camera position with `setPosition` may be very far away from the actual model. To migrate the old data you would need to translate by the `model.getGlobalOffsetDoNotUse` to be in the correct coordinate system.

For example, say you were on an old version of the Web Viewer in which `camera.getPosition/setPosition` did not operate on "world coordinates" and you were saving the camera positions with the following:

```ts
const cameraPosition = await viewer.camera.getPosition();

postCameraPositionToServer(cameraPosition);
```

And then you wanted to load these saved positions to set the initial camera position:

```ts
const { x, y, z } = await getCameraPositionFromServer();

viewer.camera.setPosition(x, y, z);
```

But then a new version of the Web Viewer is released that updates `camera.getPosition/setPosition` to be in "world coordinates". The values you have saved in your DB will now be incorrect when you pass them to `setPosition`.

To fix this issue you would need to add the global offset (`model.getGlobalOffsetDoNotUse`) to the position before calling `setPosition`:

```ts
const { x, y, z } = await getCameraPositionFromServer();
const offset = await viewer.model.getGlobalOffsetDoNotUse();

viewer.camera.setPosition(x + offset.x, y + offset.y, z + offset.z);
```

Saving to the DB would also need to be updated to keep your DB values in a consistent coordinate system:

```ts
const { x, y, z } = await viewer.camera.getPosition();
const offset = await viewer.model.getGlobalOffsetDoNotUse();

postCameraPositionToServer({
  x: x - offset.x,
  y: y - offset.y,
  z: z - offset
});
```

## Legal Notice

<p class="heading-link-container">
  <a class="heading-link" href="#legal-notice"></a>
</p>


Autodesk and Navisworks are registered trademarks or trademarks of Autodesk, Inc., in the USA and other countries.
