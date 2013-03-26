### Admin service

For administrating viewers, some request at a meta level are needed. (*In the first implementation everything will be available for everyone, except for the layers?*) In the future implementing searching and paging might be necessary (see google style guide)

Return the metadata of the tools that are available for this customer. 

- GET service/tool/{klantnaam}/kinds
- GET service/viewer/{klantnaam}/kinds
- GET service/layer/{klantnaam}/kinds


In version 0.5 all return data will be similar:


	schema({
		data : Array.of(1,500, Metadata)  	// Where the 'kind' field of the metadata
		 									// corresponds to the requested objects
											// for example Layer 
	});




### Viewer
A viewer itself also contains some metadata, ie the tools that are available in the viewer. This information is can be requested from the viewer url. 

- GET [viewerurl]/config/viewer.json 




