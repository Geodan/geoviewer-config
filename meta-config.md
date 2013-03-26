### Metadata ###

Each configuration object also has a related metadata object. The metadataobject allows the construction of GUI elements for advertising en configuring the object it is refering to.  

The general Metadata object had the following format:

	var MetaData = schema({                 // Definition of the metadata object, is returned in the configuration service
	  'kind' : [ 'Layer', 'Viewer', undefined], 	// what kind of object 
	  'id' : /[azAZ\-]*/,					// id, no spaces aloud	
	  'title' : String,                     // human readable name
	  '?thumbnail' : String,				// optional url to thumbnail
	  '?description' : String,				// Text describing the object
	  'summary' : String,					// Short abstract, for example used in tooltip
	  '?owner' : String,					// Owner of the object (IS THIS NEEDED?)
	  '?url' : String,								// url to the object 	
	  '?options' : Array.of(MetaOption)		// description of the options that the object
										// can receive
	});

Meta Option


	var MetaOption = schema({
	meta : undefined,				// TODO: there is no real need for the
									//  meta as object here, flatten it?
	'kind' : ['integer','float', 'string', 'list', 'date'],  
									// kind of the field
	'key' : String,					//
	'?maxValue' : Number,			// maximum allowed value
	'?minvalue' : Number,			// minimal allowed value
	'?step' : Number,				// step with which a number can be chosen 
	'?codelist' : Array,	 		// a codelist of values that can be chosen
	'?defaultValue' : undefined, 	// Optional default value of the option	
	'?constraint' : String			// An url for the external check of constraints TODO: is deze nodig?											
	});


Simple metadata, the light version of Metadata used in the viewer service

	var SimpleMetaData = schema {(      // metadata of the object, is returned in the viewer service
		id : /[azAZ\-]*/,				// reference to the complete metadata
		'title' : String,               // human readable name
		'?thumbnail' : String,			// optional url to thumbnail
		'summary' : String,				// Short abstract, for example used in tooltip
		'?owner' : String,				// Owner of the object
	)}