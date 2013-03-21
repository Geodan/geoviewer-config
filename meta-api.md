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

