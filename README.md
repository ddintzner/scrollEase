# ElantraBetterWhyPage-MY17
HMA-M15-AD-8944-MY17_ELANTRA_LAUNCH


scrollEase.js
------------

_A .js framework for a smooth easing scroll with rich optimized features._

#### Description
scrollEase.js handles the scrolling, easing and viewport management of a page. Features include physics based scrolling, waypoint type event calls, 
element pinning, property easing and video scroll-scrubbing. 


#### Usage
Using the scrollEase can be broken into 3 stages
1. Setting the data models
2. intializing scrollEase object
3. Handling the scrollEase event calls.


# 1. Setup - Data
Designed to handle the scope of the Elantra Why page, the framework uses multiple data models: a waypoint type that describes a module’s behavior relative to the window and an ease type which outlines the easing behavior for a particular module.  

## Waypoint Data
Key | Value | Description
------ | ------- | ------- 
el | string	| the identifier of the element in HTML
down | int or float	| on scroll down, the trigger value from element top to browser top. An int will render in pixels the amount from the top.  A  0.01 - 0.99 float value can be used as a percentage window based offset.  
up	| int or float	| on scroll up, the trigger value from element top to browser top.   An int will render in pixels the amount from the top.  A  0.01 - 0.99 float value can be used as a percentage window based offset.  
maxTriggers	| int | define how many times the element can triggers. optional - 1000 if not defined. 

### Example
Below example defines the element '[data-id="foo"]' to trigger when the top element is 40 pixels past the top edge of the browser when scrolling down.  Conversely,  it will trigger if you are scrolling up and it becomes 350px below the browser’s top.

```javascript
var foo =   {  "el" :  '[data-id="foo"]' , "down" : -40,  "up" : 350   };
```

An optional key ‘maxTriggers’ determines how many times the element will fire.  Now it will just trigger once.

```javascript
var foo =   {  "el" :  '[data-id="foo"]' , "down" : -40,  "up" : 350,   "maxTriggers" : 1 };
```

Providing a float value, such as 0.5, will trigger when the top of element is at 50% of browser height.

```javascript
var foo =   {  "el" :  '[data-id="foo"]' , "down" : 0.5,  "up" : 350 };
```


### Finishing the Waypoint Data 
An array is required when passing the waypoint data to scrollEase.

### Example
```javascript
var foo1 =   {  "el" :  '[data-id="foo1"]' , "down" : -40,  "up" : 350  };
var foo2 =   {  "el" :  '[data-id="foo2"]' , "down" : -60,  "up" : 250,   "maxTriggers" : 1 };
var foo3 =   {  "el" :  '[data-id="foo3"]' , "down" : -20,  "up" : 500  };

var fooWayPointData = [foo1, foo2, foo3];
```

‘fooWaypointData' can be passed to scrollEase.

## Ease Data
Setting the ease data branched into a few variations.


### Ease Video-type Scroll Data
Key | Value | Description
------ | ------- | ------- 
name	|		string	|	the identifier for accessing the data in the framework.
el		|	string	|	the identifier of the target element for the scrolling.
module	| string	|	the identifier of the parent target element in the HTML. 
id		|	string	|	the identifier for the video element's id to scroll.   
pinType	|	string	|	required - can be set to either  “scroll” or “fixed”.  “fixed” will pin the element.
pinOffset	|	int	| 	the value offset from the top of the browser that the element will pin
friction	|		float	|	the value that will set the physics scroll damping. Defaults to 10.
elementTopOffset  |	float	|	amount of pixels from the top of the browser that an video will start the scrub
media		|	string 	|	required - can be set to either  “css” or “video”.  “video” is used for video scroll control.
mediaProperties  | 	array   |    	a set of additional properties that describes the video media.

### Ease Video Scroll Data - MediaProperties array 
Key | Value | Description
------ | ------- | ------- 
preload	 |	boolean	| recommended true for best results
file	 |		string	|	path to the video
fps		|	float	 |	frames per second for video, defaults to 30.00
totalFrames	  |	int	 |	the  frame duration for the video


### Example
The example below defines the element '[data-id="foo"]' to trigger on scroll down if the top element moves past the browser top edge 40 pixels.

```javascript
var fooVideo  = {
    "name" : "fooVideoData",
    "module"  : ".fixedFooExample",
    "el" : ".fixedFoo-element",
    "id" :  "fooVideo",                           
    "pinType" :  "fixed",
    "pinOffset" : 50,
    "friction" : 5,                                   
    "elementTopOffset" :  400,
    "media" : "video",
    "mediaProperties" :  [{
        "preload" : true,
        "file" : "./video/foo.mp4",
        "fps" : 30.00,  
        "totalFrames" : 135
    }]
};
   
```

### Ease CSS-type Scroll Data
Key | Value | Description
------ | ------- | ------- 
name	|		string	|	the identifier for accessing the data in the framework.
el		|	string	|	the identifier of the target element for the scrolling in the HTML
module	|  string	|	the identifier of the parent target element in the HTML. 
pinType	 |  string	|	 pin behavior is either  “scroll” or “fixed”.  “fixed” will pin the element.
pinOffset	|	int	 |	the value offset from the top of the browser that the element will pin.
friction	|		float	|	the value that will set the physics scroll damping. Defaults to 10.
elementTopOffset  |	float	|	amount of pixels from the top of the browser that an video will start the scrub.
tweenDistance    |	int	 |	the amount of pixels needed for  scroll to complete the 0-1 tween ratio.
media		|	string 	|	required - can be set to either  “css” or “video”.  “css” is used for css scroll animations.
mediaProperties   |	array     |  	a set of additional properties.


### Ease CSS Scroll Data - mediaProperties array 
When setting the mediaProperties for CSS data, there is a primary, and an optional secondary. The system is set to handle the
common scenario:  a primary y or x positional and a secondary fade.  

#### PRIMARY mediaProperties  set 
Key | Value | Description
------ | ------- | ------- 
property	|	string	 |	the css propery to ease.
startProperty	|	int or float	 |	value to start.
goalProperty	|	int	or float |	value to end.
suffix		|	string	 |	property identifier, use “px” for css property that needs it.  -optional

The secondary set is similar to the primary, but  introduces an ‘offset’ property.

#### SECONDARY mediaProperties  set
Key | Value | Description
------ | ------- | ------- 
property	|	string	 |	the css propery to ease.
startProperty	|	int	or float |	value to start.
goalProperty	|	int	or float |	value to end.
offset		|	float	 |	multiplier to scale the speed of the secondary property.
suffix		|	string	 |	property identifier, use “px” for css property that needs it.  -optional

### Example
The secondary element can use an optional function called ‘offset’. This wiIl create an accelerated ease, remapped to the end section of the primary tween.   The value supplied is then used a multiplier to the difference between the startProperty and goalProperty. <br><br> Opacity is a common use for the ‘offset’ multiplier,  a fade transition can sometimes be more effective if remapped to a faster speed. This  especially useful when using a solid black text color against a solid white background. 

```javascript
 var fooCopy = {

    "name" : "fooCopy",
    "el" : ".foo-content",
    "module"  : "[data-id='fooModule5']",
    "pinType" : "scroll",
    "friction" : 10,
    "elementTopOffset" : 650,
    "tweenDistance" : 200,  
     "media" : "css",
     "mediaProperties" :  [ {
            //primary                                  
             "property" :  "margin-top",
             "startProperty" : 100,
             "goalProperty" : 30,
             "suffix" : "px"
            },  {
             // secondary CSS
             "property" :  "opacity",
             "startProperty" :  0.0,
             "goalProperty"  : 1.0,
             "offset" : 0.75               
          }]
      };

````

The ease scroll array, similar to the waypoint data array, is a required array.
Our 'fooScrollData' can be passed to scrollEase, which is detailed below.

````javascript
var fooScrollData = [fooVideo , fooCopy];
````





# 2. Initialize - scrollEase
Initialize a scrollEase instance using arrays for the scroller and the waypoint data. Additional properties include defining the rate the scroller covers per pixel, and methods for video load completion, viewport element visibility and waypoint trigger events.


Jquery +1.7 is required, and needs to be declared before scrollEase.js

```html
<script type="text/javascript" src="//code.jquery.com/jquery-1.11.0.min.js"/></script/>
<script type="text/javascript" src="js/scrollEase.min.js"/></script/>
```



### scrollEase properties

key    |  type | description
------ | ------- | ------- 
data		|	array		| the user defined scroll data.
frameScrollRatio  |	 float	|		how many pixels in height each frame needs to scroll to progress, will affect scroll-scrub videos. Defaults to  12.00.
video.loadComplete	    |     	event	| define the method for scrolling video load completion.
waypoints.data	        | 		array		|	the user defined waypoint data.
waypoints.dispatch      |		event	|	define the method when a waypoint is triggered.
waypoints.viewportFeed      |		event	|	define the listener for the viewport status stream.


Example
Below is an example outlining the initializing, using the data variables from above. 


````javascript
var myScrollEase = new ScrollEase(  {
	"data" :  fooScrollData,
	"frameScrollRatio": 10.00, 		              
	"video" : {
		"loadComplete" : function() {  myVideoCompleteLoad(); }
		 },
	"waypoints" : {
		"data" : fooWaypointData,
		"dispatch" : function() {  mydispatch(); }  
		"viewportFeed" : function() {  myviewportFeed(); }      
	 }
 });
````

# 3. Handling - Event Calls
Waypoint and video load completions are event calls made by scrollEase.  Scroll videos can be reloaded or changed.

### properties  
Instance property   			|	value  | 	context		|		description
------ | ------- | ------- | -------
instance.video.loadComplete.id	|	string	|	video.loadComplete	|	will return the id of the most recent loaded video. 
instance.waypoints.triggeredEl	|	string	|	waypoints.dispatch	|	returns the triggered element accessor's name  
instance.waypoints.moduleIndex	|	int	|	waypoints.dispatch	|	returns the array index number of the triggered waypoint
instance.waypoints.elements[n].triggerAmounts | int	 |	waypoints.dispatch    |  	returns how many times an element has been triggered
instance.waypoints.index[string]   | string	 |	waypoints.viewportFeed    |  	returns the index (int) of the named element from the waypoints data array 
instance.waypoints.elements[n].state | int	 |	waypoints.viewportFeed    |  	returns "on" or "off" if the element is in viewport



### methods  
Instance methods  	 |		parameters	 |	description
------ | ------- | ------- |
instance.videoReload(id, file) 	 |	string, string	 |	use to reload or change a scrolling video.  Supply the video id and file path.


### video.loadComplete
For scrolling videos, ‘loadComplete’ gets called after loading completes. Use the property ‘video.loadComplete.id’ on a scrollEase instance to reference the video id.  The example below turns on the opacity after the video loads.


````javascript
function  myVideoCompleteLoad() {

	console.log("Video complete load with id: " +  myScrollEase.video.loadComplete.id);

	var id = "#" + myScrollEase.video.loadComplete.id;  
	$(id).removeClass("opacity-0");

};
````

### waypoints.dispatch
Both _instance_.waypoints.triggeredEl and _instance_.waypoints.moduleIndex are accessible in the dispatch event call. They are used to filter the data when dispatched is fired.  Note,  instance.waypoints.moduleIndex can be used as the _n_ in instance.waypoints.elements[n].triggerAmounts  to return the amount of times triggered.


### Example

````javascript
function mydispatch() {

	var el =  myScrollEase.waypoints.triggeredEl;
	var m =   myScrollEase.waypoints.moduleIndex;
	var triggereHits = myScrollEase.waypoints.elements[m].triggerAmounts;
		 
	console.log( " module: " + m  + "   el : " + el  + " hits: " +  triggereHits );

	if( el == '[data-id="foo-video-4"]'    ) {
		$("#myFooV").get(0).play();
	}


	if( el == '[data-id="fooThingtoAnimate"]'  ) {

		//trigger animation
		$( ".foothing" ).animate({
			left: "490", 
			}, 2000, function() {
 			// Animation complete.
		});
	}
};

````

### waypoints.viewportFeed
An important feature of waypoints is the ability to determine if an element is presently in view.  Triggered on scroll, you have the ability to filter a data stream by checking an element's boolean state.
The example below will play the video if it is in view, otherwise it will pause.  

### Example

````javascript
function myviewportFeed() {
	 	  
	 //get the element by using the 'index' property
	 	  	
     var i = myScrollEase.waypoints.index["myVideo"];
     var el = myScrollEase.waypoints.elements[i]].el
     
     if( el.state && $(el).paused )
     	$(el).get(0).play();
   
     if( !el.state  &&  !$(el).paused )
     	$(el).get(0).pause();
     	
};

````


#### Browser support

scrollEase is supported on modern browsers Chrome, Firefox, IE and Safari.

#### Dependencies

jQuery 1.7

#### License

Copyright (c) 2017 INNOCEANUSA

Licensed under the MIT license.

