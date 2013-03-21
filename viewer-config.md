###  Objecten 

Er zijn een aantal configuratie objecten, al deze objecten gecombineerd vormen het geoViewer object:

- Layer
- Map
- geoViewer

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



####opties voor een Layer object:

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


