**status**: *proposal*

### Recomendations for client implementations

A general guideline for implementation of a viewer is described here.

A viewer is UI representation of a map. A viewer can be an actual viewer displaying a map in the browser on the users computer but it can also be an service, for example for display of the data in Layar on a mobile phone.

A viewer needs to implement the following:

##### viewer url

Each viewer should provide the cloud configuration server with an url.

Example:

	http://geoviewer.nl/mv/

##### viewer.config - meta viewer configuration information

The subdirectory *config* of the provided url should contain a file named *viewer.config* 

Example:

	http://geoviewer.nl/mv/config/viewer.config


Viewer.config contains the meta information of the viewer and has the following format:


	var Rights = schema({
	   usage: ["private", "shared", "public"],
	   '?groups' : Array.of(String)
	})

(rights description should move to a more general page.
	
	var MetaViewer = schema({
		version: String.of('0-9\.'),
		apiVersion: String.of('0-9\.'), // version of the api this config file complies to
		meta: MetaData,					// contains a description of the viewer
										// is provided to the user in the userinterface
		tools : Array.of(String.of('a-zA-Z0-9\-')), // array of ids. Each id represents a tooltype 
										// which is supported by this viewer
		rights : Rights					// which users have access to this viewer								
	});	
	
	
[An example of a viewer.config file.](example-client-viewer-config.md)	
	
The viewer.config file makes it possible to provide the user with an interface that contains information of the capablities of the viewer provided. 	


###### viewerid

The subdirectory *viewer* should accept a parameter *viewerid*. A call to this url should open the viewer using the configuration that corresponds to the provided viewerid. In case of non-webapplication, for example a native IOS application, the link should open either a forward to the appstore or a page explaining what the user has to do to use the viewer.

The viewer configuration should be requested by the viewer using the viewerid see [viewer-api documentation](viewer-api.md).

Example:

	http://geoviewer.nl/mv/viewer/?viewerid

