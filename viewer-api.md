**status**: *draft*


## geoViewer Configuration Service ##

De geoViewer configuratie service voorziet in het opslaan en verkrijgen van configuraties. Deze configuraties worden gebruikt voor het samenstellen en weergeven van kaarten en viewers. Een configuratie is eigendom van een persoon en kan door die persoon gedeeld worden met zichzelf, zijn bedrijf of de wereld. 



### Viewer Service

#### edit viewer configuration: ####

- GET service/viewer/{klantnaam}/{viewernaam}
- PUT service/viewer/{klantnaam}/{viewernaam}
- DELETE service/viewer/{klantnaam}/{viewernaam}

#### list viewers: ####

	var ViewerList= schema({
		viewers : Array.of({
			'id' : String.of('a-zA-Z0-9'),
			'?title'	: String,				// human readable title of the map
			'?description' 	: String,				// Short abstract describing the map
			'?thumbnail'   	: String				// url naar png
	});

- GET service/viewer/{klantnaam}/

