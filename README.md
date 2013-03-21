geoviewer-config
================

Geodan's geoviewer configuration API. 

Bevat de draft die moet leiden tot de API definitie die voor de geoViewer cloud applicatie van Geodan.

-------------------





Admin

- achtergrondkeuze
- extent
- extra layer
- controls 

TODO:

 - viewer onderscheiden IOS, OL etc. 
 - GIThub zetten


[Example configuration](https://github.com/Geodan/geoviewer-config/blob/master/example-config.json)


## geoViewer Configuration Service ##

De geoViewer configuratie service voorziet in het opslaan en verkrijgen van configuraties. Deze configuraties worden gebruikt voor het samenstellen en weergeven van kaarten en viewers. Een configuratie is eigendom van een persoon en kan door die persoon gedeeld worden met zichzelf, zijn bedrijf of de wereld. 

###  Objecten 

Er zijn een aantal configuratie objecten, al deze objecten gecombineerd vormen het geoViewer object:

- Layer
- Map
- geoViewer

There is not a strong division between visualisation and configuration. If you need another visualisation, simply define another configuration


### xxx###

De [google json style guide](http://google-styleguide.googlecode.com/svn/trunk/jsoncstyleguide.xml) wordt, waar toepasbaar, als richtlijn gebruikt. 

Voor de beschrijving van de json structuur wordt gebruik gemaakt van [js-schema](http://molnarg.github.com/js-schema/). Js-schema is leesbaar voor mensen, kan worden gebruikt voor het genereren van voorbeeld data, het valideren van input data  en is vertaalbaar naar JSON SCHEMA.

De documentatie wordt beschreven met behulp van markdown.

De voertaal is engels :)


### Vragen ###

- is 1 versie op api level voldoende, of moet elk object zijn eigen versie nummer hebben


### Metadata ###

Each configuration object also has a related metadata object. The metadataobject allows the construction of GUI elements for advertising en configuring the object it is refering to.  

The general Metadata object had the following format:

  var MetaData = schema({                 // Definition of the metadata object
	  'kind' : [ 'Layer', 'Viewer', *], 	// what kind of object 
	  'id' : /[azAZ]*/,						// id, no spaces aloud	
	  'title' : String,                     // human readable name
	  '?thumbnail' : String,				// optional url to thumbnail
	  '?description' : String,				// Short abstract describing the map
	  'summary' : String,					// Short abstract, for example used in tooltip
	  '?owner' : String,					// Owner of the object (IS THIS NEEDED?)
	  '?url',								// url to the object 	
	  '?options' : Array.of(MetaOption)		// description of the options that the object
											// can receive
	});

	var MetaOption = schema({
		meta : Metadata,				// TODO: there is no real need for the
										//  meta as object here, flatten it?
		'kind' : ['integer','float', 'string', 'list', 'date'],  
										// kind of the field
		'?maxValue' : Number,			// maximum allowed value
		'?minvalue' : Number,			// minimal allowed value
		'?step' : Number,				// step with which a number can be chosen 
		'?codelist' : Array,	 		// a codelist of values that can be chosen
		'?defaultValue' : *, 			// Optional default value of the option	
		'constraint' : String			// An url for the external check of constraints TODO: is deze nodig?											
	});

	

### Layer ###

The layer object describes both the “data” that needs to be represented as the way the layer should be represented in a viewer. The openlayers configuration is used where applicable (and generic enough).

An Option is a key value pair. An option contains meta information describing the valid values, and the goal  of the option.


	var Option = schema({						
		'meta' : MetaOption, // TODO: or should this be the metaid instead of the obejct?
		'?value' : *		// Optional when meta.defaultValue exists else obligated 
	}); 

The Layer object syntax
	
	var Layer = schema({                    	// Definition of the layer object
	  'meta' : MetaData,						// meta data of the layer	
	  'kind' : [ 'WMS', 'WMTS', 'Datalayer'], 	// Laagsoort zie laagtypen TODO: this is a double of the kind in MetaData?
	  'id' : /[azAZ]*/,						   	// id van de laag, no spaces aloud	
	  '?title' : String,                        // human readable name, overrides the meta.title value
	  '?url' : String,						   	// optional (depending on kind) url to the service where this object can be retrieved
	  'params' : {}						   	   	// parameters for example
												// when WMS: the url parameters
	  'options' : Array.of(Option)				// options that are applicable for visualising the layer by the viewer
	});

Following code describes a WMS object. Only part of the implemented Options are described. There will be no formal description of each layer kind. Instead the generated admin interface of each layer will function as the description of the layer options.


     // example of a wms layer object
	var wmslayer = {
		meta : {
			kind : "Layer",
			id : "layer-geodak",
			title : "geodak",
			summary : "",
			description : "",
			thumbnail : "",
			options : [
				{
					meta : {
						meta : {
							kind : "Option",
							id : "option-layer-wms-transparency",
							title : "Transparency",
							summary : "The transparantie van de laag. (1 = volledig transparant.)",
							description : "The transparantie van de laag bepaald de mate waarin onderliggende lagen zichtbaar zijn door deze laag heen. Een transparantie van 1 laat de achterliggende lagen volledig zien, effectief wordt de laag dus onzichtbaar. Een transparantie van 0 toont niets van de achterliggende lagen.",
							thumbnail : "http://linggis.geodan.nl/pictures/option-transparency.png"
						},
						kind : float,
						maxValue : 1, 
						minvalue : 0,
						step : 0.1,
						defaultValue' : 0
					}
				},
				{
					meta : {
						meta : {
							kind : "Option",
							id : "option-layer-wms-weight",
							title : "Gewicht",
							summary : "Het gewicht van de laag. (100 is onder, 0 is boven)",
							description : "Het gewicht van de laag bepaald de getoonde volgorde van de lagen. De getoonde volgorde wordt bepaald door het relatieve gewicht ten opzicht van elkaar. Hoe zwaarder de laag hoe lager deze in de kaart wordt weergegeven. ",
							thumbnail : "http://linggis.geodan.nl/pictures/option-weight.png"
						},
						kind : float,
						maxValue : 100, 
						minvalue : 0,
						step : 1,
						defaultValue' : 50
					}
				},
				....    // a lot more otions....	

			]	 	
		}
		kind: "WMS",
		id: "mylayer23",
		title : "myOwntitle",
		url : "http://194.45.159.205:81/wms?",
		params : {
			map : "geodak-tst.map",
			version : 1.1.1,
			layer: "panden",
			style : "light"
		}
		options : {
			visible : true,
			weight : 5,
			transparency : 0.2,
			maxZoomlevel : 5,	
			minZoomlevel : 2,
			info : true,
			in	
		}	
	};

 
For readability options are described here as js-schema, actually options is an ordered collection of Option 
	
	var Options - schema({						// wat leggen we hier wel of niet vast?
		'visible' : Boolean,						// initial visibility
		'weight' : Number.min(0).max(100),			// 0 is shown as bottom layer
		'transparency: Number.min(0).max(1)      // initial transparancy of the layer
	'maxZoomlevel' : Number,
	'minZoomlevel' : Number
	'info' : boolean		
	});

### Map ###
	var GeoMap = schema({
		'?title'	: String					// human readable title of the map
		'?description' : String				// Short abstract describing the map
		'id' : String.of([a-zA-Z0-9]),		// identifier of map
		'layers' : Array.of(1,500,Layer),	// 
		'?initialScale' : Number,			// initial scale of the map (initialViewport will take precedence)
		'?initialCenter' : { 				// initial center of the map (initialViewport will take precedence)
			x: Number, 
			y: Number 
		},			 
		'?initialViewport' : { 	// either initial Bbox or scale and position van be used
			'top' : Number,
			'left' : Number,
			'right' : Number,
			'bottom' : Number
		 }, 
		'srs' : /EPSG:[0-9]*/,						// The projection of the map 
		'resolutions' : [1092,			// ? Of moet dit bepaald worden door de lagen?
				364,
				122,
				40.5,
				13.5,
				4.5,
				1.5,
				0.5,
				0.1666666667],
		'?maxExtent' : {				// maximum extent that the viewer will shows
					'left' : Number,
					'right' : Number,
					'top' : Number,
					'bottom' : Number
				},
		'?proxy': "proxy.jsp?"		  // relative path to a proxy script 	
		
	});


### Viewer ###


	var Viewer = schema({
		'kind' : ['ol','mapsui','integrated'], 	//
		'?title'	: String,						// human readable title of the map
		'?description' : String,				// Short abstract describing the map
		'id' : String.of('a-zA-Z0-9'),			// identifier of map
		'maps' : Array.of(1,100,geoMap),		// ? Of moeten dit referenties naar Maps zijn?
		'tools' : Array.of(Tool),		// Not every kind of viewer, supports all tool (or all options)
									// viewer will ignore when unkown
		
	});

### Tool ###
	var Tool = schema({
		'meta' : MetaData,
		'id' : String.of('a-zA-Z0-9'),		// identifier
		
		'?activated' : Boolean,
		'?visible' : Boolean,
		'?inMenu' : Boolean,
		'?asButton' : Boolean,
		'options' : Array.of(Option)		// options that are specific for the tool of kind, 
											/// not all viewers will support all options
 
	});




### REST services urls ###

? kijk even naar google style guide 

All PUT and DELETE request should be available in a POST and GET wrapper, to ease crossdomain access  


#### Response Envelop ###

All restservice use the following envelop

	var Response = schema({
		apiVersion : “1.0.12”,     // a.b.c if the json object changes increase
								// a) the name of a field or its location
								// b) a parameter is added to the json 
								// c) might be adjusted to indicate service version
		status : ["success", "failure", "error"], // status of the response
		?data : {…}, 				// the result of the response, obligated when state==succes
		?error : {…}  			// error description, obligated when state!=failure 
	});

### Viewer Service

#### edit viewer configuration: ####

- GET service/viewer/{klantnaam}/{viewernaam}
- PUT service/viewer/{klantnaam}/{viewernaam}
- DELETE service/viewer/{klantnaam}/{viewernaam}

#### list viewers: ####

	var ViewerList= schema({
		viewers : Array.of(1,500,{
			'id' : String.of('a-zA-Z0-9'),			
			'kind' : ['ol','mapsui','integrated'], //
			'?title'	: String,					// human readable title of the map
			'?description' : String,				// Short abstract describing the map
	});

- GET service/viewer/{klantnaam}/


### Admin service

For administrating viewers, some request at a meta level are needed. (*In the first implementation everything will be available for eveyone, except for the layers?*)

Return the metadata of the tools that are available for this customer. 

- GET service/tool/{klantnaam}/kinds
- GET service/viewer/{klantnaam}/kinds
- GET service/layer/{klantnaam}/kinds

In version 0.5 alle metadata will be similar:

	var Opt = some kind of meta data describing a field in a form including tooltip etc. 

	var Metadata = schema({
		viewers : Array.of(1,500,{
			'id' : String.of('a-zA-Z0-9'),			
			'name'	: String,				// human readable name
			'description' : String,			// Short abstract describing the thing
			'options' : Array.of(Opt)    // use a standard mechanisme to describe a form
	});


### Recomendations for client implementations

A general guideline for implementation is described here

- to make it easier for developers to switch between development in different clients
- easier maintenance
- allow reuse in "maakwerk" applications that connect to the cloud

Objects with the following name and working should be defined:

- gvConfigViewer
	- parameters: viewername
	- returns: an object representing the viewer configuration  
	- uses: gvConfigMap, gvConfigTool
- gvConfigMap
- *gvCongfigTool*, can/should be provided by the cloud service??

Crossdomain communication should be supported where possible. 



### Vragen ###

- Hoe gaan we om met meertaligheid? Moet hier al rekening mee gehouden worden...

