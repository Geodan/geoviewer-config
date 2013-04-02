**status**: *draft*


geoviewer-config
================

Geodan's geoviewer configuration API. 

Bevat de draft die moet leiden tot de API definitie die voor de geoViewer cloud applicatie van Geodan.

-------------------

- [Viewer configuratie definitie](viewer-config.md) 
- [Viewer configuratie voorbeeld](example-viewer-config.json)
- [Viewer service REST API](viewer-api.md)
- [Viewer implementation](viewer-guidelines.md) - What to implement to create a new viewer

- [Meta configuratie definitie](meta-config.md)
- [Meta configuratie voobeeld](example-meta-config.json)
- [Configuratie service REST API](meta-api.md)




POC definitie
=============

Voor eerste versie viewer is gekozen voor het meest simpele voorbeeld als Proof of Concept en als goede basis voor verdere ontwikkeling.

In de viewer Admin kunnen de volgende onderdelen worden geconfigureerd.

- achtergrondkeuze
- extent
- extra layer
- controls 

Als Layer kind wordt gekozen voor WMTS.


- viewer meta data definieren 

There is not a strong division between visualisation and configuration. If you need another visualisation, simply define another configuration


### xxx ###

De [google json style guide](http://google-styleguide.googlecode.com/svn/trunk/jsoncstyleguide.xml) wordt, waar toepasbaar, als richtlijn gebruikt. 

Voor de beschrijving van de json structuur wordt gebruik gemaakt van [js-schema](http://molnarg.github.com/js-schema/). Js-schema is leesbaar voor mensen, kan worden gebruikt voor het genereren van voorbeeld data, het valideren van input data  en is vertaalbaar naar JSON SCHEMA. De json beschrijft de API communicatie, het is NIET een omschrijving van de interne data structuur.

De documentatie wordt beschreven met behulp van markdown.

De voertaal is engels :)


### REST API ###

#### REST services urls ####

? kijk even naar google style guide 

All PUT and DELETE request should be available in a POST and GET wrapper, to ease crossdomain access  


##### Response Envelop ####

All restservice use the following envelop

	var Response = schema({
		apiVersion : �1.0.12�,     // a.b.c if the json object changes increase
								// a) the name of a field or its location
								// b) a parameter is added to the json 
								// c) might be adjusted to indicate service version
		status : ["success", "failure", "error"], // status of the response
		?data : {�}, 				// the result of the response, obligated when state==succes
		?error : {�}  			// error description, obligated when state!=failure 
	});

#### Generic JSON objects

Some JSON objects are shared between different objects and are described here:

##### Option

An Option decribes a key value pair. An option contains meta information describing the valid values, and the goal of the option.

	var Option = schema({						
		'meta' : MetaOption, // TODO: or should this be the metaid instead of the object?
		'?value' : undefined		// Optional when meta.defaultValue exists else obligated 
	});  
	
For simplicity an Option is represented as a key-value pair in the viewer service ie:

	options: { 
		key1: value, 
		key2: value2 
	}

	

### Vragen/ TODO ###

- is 1 versie op api level voldoende, of moet elk object zijn eigen versie nummer hebben

- hoe om te gaan met projectie van kaart en lagen; liefst geeft server bij een kaart met gekozen projectie (zeg RD) en extent/middelpunt/schaal alleen de geschikte lagen terug

