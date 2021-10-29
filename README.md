# tailormate3d-lib-esm

Website [_Tailormate3d_](https://www.tailormate3d.com)

## Initialize Viewport

```js
var viewport = new Tailormate3d(options); 
```

### Parameters

-   `options` **[Object][5]**
    -   `options.container` **[HTMLElement][7]** `document.getElementById('container')` (optional, default : document.body)
    -   `baseUrl` **[String][1]?** Server URL (Required) - `https://tailormate3d.com/{instanceName}/`
    -   `apikey` **[String][1]?** API Key required to access the data from server (Required)
    -   `backgroundTransparent` **[Boolean][6]** Enable/Disable background(optional, default `false`)
    -   `options.offsetWidth` **[Number][8]** canvas element width (optional, default : Container element width)
    -   `options.offsetHeight` **[Number][8]** canvas element height (optional, default : Container element height)
    - `backgroundColor` **[Color](#tm-color)**
    - `floor` **[Object][5]** (Optional)
        - `enabled`  **[Boolean][6]** Enable/Disable(optional, default `true`)
        - `color` **[Color](#tm-color)** (optional, default `#ffffff`)
    - `fog` **[Object][5]** (Optional)
        - `enabled`  **[Boolean][6]** Enable/Disable(optional, default `true`)
        - `color` **[Color](#tm-color)** (optional, default `#a0a0a0`)
    -   `model` **[Object][5]** (Optional)
        - `position` **[Object][5]?** (Optional)
            - `x`  **[Number][8]** position x (optional)
            - `y`  **[Number][8]** position y (optional)
            - `z`  **[Number][8]** position z (optional)
        - `scale`  **[Number][8]** Object scale (optional)

#### Return **ViewportObject** 


## Initialize Color <a name="tm-color"></a>

### Parameters

-   `color` Hex color

Example:

```js
var gray = new Tailormate3d.Color(0xa0a0a0)
var red = new Tailormate3d.Color('#ff0000')
```


## Initialize Scene <a name="tm-scene"></a>

### Parameters

-   `sceneReferenceId` **[String][1]?** Reference id of scene (Required) - _provided in config file_
-   `type` **[String][1]?** Type of scene (Required) - _Custom identifier of the scene_
-   `options` **[Object][5]**
    - `resetPosition`  **[Boolean][6]** If you set true then model position will reset when load complete. _[This could be use with initial load]_ (optional, default `false`)
    - `isHidden`  **[Boolean][6]** If you set true then model will load but it will not display on viewport. You can manually show model once model will load. _[This could be use with initial load]_ (optional, default `false`)

Example:

```js
const opt = {
    resetPosition:true,
    isHidden:false
};
var scene = new Tailormate3d.Scene('shirt_cuff_one_button_rounded','cuff',options)
```


### Functions

## resize

Resize the viewport

#### Parameters

-   `width` **[Number][8]?** (Required)
-   `height` **[Number][8]?** (Required)

Example:

    viewport.resize(width,height);

## loadScene

Load 3d scene into the viewport

#### Parameters

-   `sceneReferenceId` **[String][1]?** Reference id of scene (Required) - _find in scene config_
-   `options` **[Object][5]?** (required in case of callback)
    -   `type` **[Number][8]** change the scale of object - Float (optional)
-   `success` **[Function][2]?** called on completion with one arguments `info` contains the scene data.
-   `error` **[Function][2]?** called on error with one arguments `error`.

#### Return **[Promise][4]** 

Example:

```js
var options = {
    type: 'collar'
};
viewport.loadScene('spread_collar',options,function(info){

}, function() {
    //Error
});
```

Load multiple scenes together:
```js
var opt = {
    resetPosition:true,
    isHidden:false
};
var scenes = [
    new Tailormate3d.Scene('shirt_cuff_one_button_rounded','cuff',opt),
    new Tailormate3d.Scene('shirt_bottom_normal_curved','bottom',opt),
    new Tailormate3d.Scene('shirt_bottom_normal_curved_back_no_pleat','back',opt),
    new Tailormate3d.Scene('shirt_sleeve_long','sleeves',opt),
    new Tailormate3d.Scene('shirt_collar_classic_spread','collar',opt),
    new Tailormate3d.Scene('shirt_front_placket','placket',opt),
    new Tailormate3d.Scene('shirt_regular_yoke','yoke',opt),
    new Tailormate3d.Scene('shirt_pocket_diamond_left','pocket',opt),
]
viewport.loadScene(scenes,
    opt,
    function onComplete(info){
        console.log('Complete', info);
    },
    function onError(err){
        console.log(err)
    }
)

// You can use it using promise
viewport.loadScene(scenes)
.then(function(info) {
    console.log('Complete', info);
})
.catch(function(err){
    console.log(err)
})
```

## removeScene
Remove scene by scene id
#### Parameters
-   `sceneReferenceId` **[String][1]?** Reference id of scene (Required) - _find in scene config_

Example:
```js
viewport.removeScene('shirt_cuff_one_button_rounded');
```

## removeSceneByType
Remove scene by scene id
#### Parameters
-   `type` **[String][1]?** Type you defined when scene load (Required)

Example:
```js
viewport.removeSceneByType('cuff');
```

## getSceneObjects

Get list of objects loaded on the viewport

#### Return **[Array][9]** 

`array` **[Array][9]?** Array of objects

- `color` **[Color](#tm-color)**
- `name` **[String][1]**
- `scene_id` **[String][1]**
- `type` **[Boolean][6]**
- `hidden` **[Boolean][6]**
- `texture` **[String][1]**

Example:
    
    viewport.getSceneObjects();

Sample return:

```js
[{
    'color': { r: 1, g: 1, b: 1 }
    'name': "shirt_pocket_diamond_left"
    'scene_id': 'shirt_pocket_diamond_left',
    'hidden': false,
    'texture': 'https://tailormate3d.com/xx-texture.jpg'
},{
    'color': { r: 1, g: 1, b: 1 },
    'name' : 'shirt_bottom_normal_curved',
    'scene_id': 'spread_collar',
    'hidden': false,
    'texture': 'https://tailormate3d.com/xx-texture.jpg'
}]
```

## setSceneObjects

Update object scene id, type, visibility, color, texture

#### Parameters

-   `array` **[Array][9]** The array return by function `viewport.getSceneObjects()`

#### Return **[Promise][4]**

Example:
    
```js
viewport.getSceneObjects([{
    'color': { r: 1, g: 1, b: 1 }
    'name': "shirt_pocket_diamond_left"
    'scene_id': 'shirt_pocket_diamond_left',
    'hidden': false,
    'texture': 'https://tailormate3d.com/xx-texture.jpg'
},{
    'color': { r: 1, g: 1, b: 1 },
    'name' : 'shirt_bottom_normal_curved',
    'scene_id': 'spread_collar',
    'hidden': false,
    'texture': 'https://tailormate3d.com/xx-texture.jpg'
}]);

// or
var objects = viewport.getSceneObjects()
for(var i = 0; i < objects.length; i++){
    objects[i].texture = 'https://tailormate3d.com/xx-texture-2.jpg'
}
viewport.getSceneObjects(objects).then(function(){
    console.log('Complete');
});
```

## hideScene

Hide scene on the viewport

#### Parameters

-   `sceneId` **[String][1]?** Scene id (Required)

Example:
    
    viewport.hideScene('{sceneId}');


## showScene

Show the hidden scene on the viewport

#### Parameters

-   `sceneId` **[String][1]?** Scene id (Required)

Example:
    
    viewport.showScene('{sceneId}');

## removeScene

Remove scene from the viewport

#### Parameters

-   `sceneId` **[String][1]?** Scene id (Required)

Example:
    
    viewport.removeScene('{sceneId}');


## hideObject

Hide particular scene object on the viewport

#### Parameters

-   `objectId` **[String][1]?** Object id (Required) - _find in getListOfObjectsLoaded function_

Example:
    
    viewport.hideObject('{objectId}');


## showObject

Show the previous hidden scene object on the viewport

#### Parameters

-   `objectId` **[String][1]?** Object id (Required) - _find in getListOfObjectsLoaded function_

Example:
    
    viewport.showObject('{objectId}');

## removeObject

Remove object from the viewport

#### Parameters

-   `objectId` **[String][1]?** Object id (Required) - _find in getListOfObjectsLoaded function_

Example:
    
    viewport.removeObject('{objectId}');
    


## flipBack

Show the back part of the scene

Example:

    viewport.flipBack();

## resetPosition

Reset the position of all scenes loaded on viewport

Example:

    viewport.resetPosition();


## loadTextureBySceneId

Render the texture on all the objects of the scene

#### Parameters

-   `imageUrl` **[String][1]?** Design image url `https://tailormate3d.com/{instanceName}/texture/{referenceId}`
-   `sceneId` **[String][1]?** Scene id
-   `options` **[Object][5]?** (required in case of callback)
    - `shininess` **[Number][8]** Increase/Decrease shine - float 1 to 1000 (optional, default : 1)
    - `textureSize` **[Number][8]** Repeat image on model - float 0.01 to 100 (optional, default : 1)
    - `opacity` **[Number][8]** change the transparency of model - float 0 to 1 (optional, default : 1)
    - `rotate` **[Number][8]** rotate model - 0 to 360 (optional, default : 1)
    - `color` **[Color](#tm-color)**
-   `onSuccess` **[Function][2]?** called on completion with no argument.
-   `onError` **[Function][2]?** called on fail to load with one argument `(error)`. **[Error][3]?**
-   `onProgress` **[Function][2]?** called on progress to load with one argument `(progress)`. **[Progress][8]?**
#### Return **[Promise][4]** 

Example:

```js
var url = 'https://tailormate3d.com/{instanceName}/texture/{referenceId}';
var options = {};
viewport.loadTextureBySceneId(url,'spread_collar',options,function(){
    //Call on success
}, function(error){
    //Call on error
},function(progress){
    //Call on progress
});

// or
viewport.loadTextureBySceneId(url,'spread_collar').then(function(){
    //Success
})
```


## loadTextureByName

Render the texture on specific scene object

#### Parameters

-   `imageUrl` **[String][1]?** Design image url `https://tailormate3d.com/{instanceName}/texture/{referenceId}`
-   `sceneId` **[String][1]?** Object id
-   `options` **[Object][5]?** (required in case of callback)
    - `shininess` **[Number][8]** Increase/Decrease shine - float 1 to 1000 (optional, default : 1)
    - `textureSize` **[Number][8]** Repeat image on model - float 0.01 to 100 (optional, default : 1)
    - `opacity` **[Number][8]** change the transparency of model - float 0 to 1 (optional, default : 1)
    - `rotate` **[Number][8]** rotate model - 0 to 360 (optional, default : 1)
    - `color` **[Color](#tm-color)**
-   `onSuccess` **[Function][2]?** called on completion with no argument.
-   `onError` **[Function][2]?** called on fail to load with one argument `(error)`. **[Error][3]?**
-   `onProgress` **[Function][2]?** called on progress to load with one argument `(progress)`. **[Progress][8]?**

#### Return **[Promise][4]** 

Example:

```js
var url = 'https://tailormate3d.com/{instanceName}/texture/{referenceId}';
var options = {};
viewport.loadTextureByName(url,'spread_collar',options,function(){
    //Call on success	
}, function(error){
    //Call on error
},function(progress){
    //Call on progress
});

// or

viewport.loadTextureBySceneId(url,'spread_collar').then(function(){
    //Success
})
```


## loadColor

Render the color on the scene or scene object

#### Parameters

- `color` **[Color](#tm-color)** Color object (required)
- `name` **[String][1]** Scene id or object id (required)
- `material` **[String][1]** Available options `glossy`/`plastic`/`glass` (optional)

#### Return **[Promise][4]** 

Example:

```js
var color = new Tailormate3d.Color('#ff0000');
viewport.loadColor(color, 'front_button', 'plastic').then(function(){
    //Success
})
```

## toImage

Return image of viewport

#### Return **[String][1]** 
Base64 encoded url

Example:

```js
viewport.toImage();
```

## getImageBlob

Return image of viewport

#### Return **[Blob][10]** 
Image Blob

Example:

```js
viewport.getImageBlob();
```

## startRotation
Enable model auto rotation

#### Parameters

- `speed` **[Number][8]** float (optional)

Example:

```js
viewport.startRotation();
```
## stopRotation
Disable model auto rotation

Example:

```js
viewport.stopRotation();
```

## toggleRotation
Enable/Disable model auto rotation

#### Parameters

- `speed` **[Number][8]** float (optional)

Example:

```js
viewport.toggleRotation();
```

## getPosition
Get position of model

#### Return

- `object` **[Object][5]**
    - `x1` **[Number][8]** float
    - `y1` **[Number][8]** float
    - `z1` **[Number][8]** float
    - `x2` **[Number][8]** float
    - `y2` **[Number][8]** float
    - `z2` **[Number][8]** float

Example:

```js
viewport.getPosition();
```

## setPosition
Set position of model

#### Parameters

- `object` **[Object][5]**
    - `x1` **[Number][8]** float
    - `y1` **[Number][8]** float
    - `z1` **[Number][8]** float
    - `x2` **[Number][8]** float
    - `y2` **[Number][8]** float
    - `z2` **[Number][8]** float

Example:

```js
viewport.setPosition({
    x1: 0,
    y1: 0,
    z1: 0,
    x2: 0,
    y2: 0,
    z2: 0,
});
```

[1]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[2]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[3]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error

[4]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise

[5]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[6]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[7]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement

[8]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[9]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

[10]: https://developer.mozilla.org/en-US/docs/Web/API/Blob
