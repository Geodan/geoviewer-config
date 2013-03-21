## geoViewer Configuration Service ##

De geoViewer configuratie service voorziet in het opslaan en verkrijgen van configuraties. Deze configuraties worden gebruikt voor het samenstellen en weergeven van kaarten en viewers. Een configuratie is eigendom van een persoon en kan door die persoon gedeeld worden met zichzelf, zijn bedrijf of de wereld. 


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

