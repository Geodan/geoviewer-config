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

Meta Option


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

