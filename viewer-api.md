## geoViewer Configuration Service ##

De geoViewer configuratie service voorziet in het opslaan en verkrijgen van configuraties. Deze configuraties worden gebruikt voor het samenstellen en weergeven van kaarten en viewers. Een configuratie is eigendom van een persoon en kan door die persoon gedeeld worden met zichzelf, zijn bedrijf of de wereld. 



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

