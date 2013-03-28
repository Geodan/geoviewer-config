**status**: *draft*


###  Objecten 

Er zijn een aantal configuratie objecten, al deze objecten gecombineerd vormen het geoViewer object:

- Layer
- Map
- geoViewer

### Layer ###

The layer object describes both the “data” that needs to be represented as the way the layer should be represented in a viewer. The openlayers configuration is used where applicable (and generic enough).

An Option is a key value pair. An option contains meta information describing the valid values, and the goal of the option.


	var Option = schema({						
		'meta' : MetaOption, // TODO: or should this be the metaid instead of the object?
		'?value' : undefined		// Optional when meta.defaultValue exists else obligated 
	});  

The Layer object syntax
	
	// een laag in het configuratie scherm kan vele malen gekozen worden. doro verschillende gebruikers, bv de gemeentelaag en vervolgens voorzien worden van een eigen styling, hoe behouden we de link naar de originele laag definitie zodat deze aangepast kan worden, bv voor een aanpassing in de WMS service
	var Layer = schema({                    	// Definition of the layer object
		'kind' : [ 'WMS', 'WMTS', 'Datalayer'], 	// Laagsoort zie laagtypen TODO: this is a double of the kind in MetaData?
		'meta' : [MetaData, SimpleMetaData],		// meta data of the layer	
		'id' : String.of('a-zA-Z0-9\-'),		// identifier
		'?title' : String,                        // human readable name, overrides the meta.title value
		'url' : String,			   	// url to the service where this object can be retrieved
		'params' : {/[.]*/  : /[.]*/},		 // parameters (key value pairs) for example
												// when WMS: the url parameters
		'options' : {/[.]*/  : /[.]*/}		// options that are applicable for visualising the layer by the viewer
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



####opties voor een Layer object:

For readability options are described here as js-schema, actually options is an ordered collection of Option 
	
	var Options - schema({						// wat leggen we hier wel of niet vast?
		'visible' : Boolean,						// initial visibility
		'weight' : Number.min(0).max(100),			// 0 is shown as bottom layer
		'transparency: Number.min(0).max(1),      // initial transparancy of the layer
		'maxZoomlevel' : Number,
		'minZoomlevel' : Number,
		'info' : boolean		
	});



### Map ###
	var GeoMap = schema({
		'?title'	: String,					// human readable title of the map
		'?description' : String,				// Short abstract describing the map
		'id' : String.of('a-zA-Z0-9\-'),		// identifier
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
		'resolutions' : Array.of(float),			// ? Of moet dit bepaald worden door de lagen?
				
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
		'meta' : MetaData,				
		'id' : String.of('a-zA-Z0-9\-'),		// identifier
		'maps' : Array.of(1,100,GeoMap),		// ? Of moeten dit referenties naar Maps zijn?
		'tools' : Array.of(Tool)		// Not every kind of viewer, supports all tool (or all options)
									// viewer will ignore when unkown
	
	});

### Tool ###
	var Tool = schema({
		'meta' : MetaData,
		'id' : String.of('a-zA-Z0-9\-'),		// identifier
	
		'?activated' : Boolean,				// The tool is active, for example info will be returned
											// by info tool after click on map
		'?visible' : Boolean,				// The tool is visible, for example window showing returned
											// information of info tool is shown on screen at startup
		'?inMenu' : Boolean,				// The tool can be activated/deactivated in a menu
		'?asButton' : Boolean,				// The tool can be activated/deactivated using a button
		'options' : Array.of(Option)		// options that are specific for the tool of kind, 
											/// not all viewers will support all options
	});


