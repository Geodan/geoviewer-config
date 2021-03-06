**status**: *proposal*

### Recomendations for client implementations

A general guideline for implementation of a viewer is described here.

A viewer is UI representation of a map. A viewer can be an actual viewer displaying a map in the browser on the users computer but it can also be an service, for example for display of the data in Layar on a mobile phone.

A viewer needs to implement the following:

##### viewer url

Each viewer should provide the cloud configuration server with an url.

Example:

	http://geoviewer.nl/application/



###### viewerid

The subdirectory *viewer* should accept a parameter *viewerid*. A call to this url should open the viewer using the configuration that corresponds to the provided viewerid. In case of non-webapplication, for example a native IOS application, the link should open a forward to the appstore or start the application with the proper id or a page explaining what the user has to do to use the viewer.


Example:

	http://geoviewer.nl/application/viewer/?viewerid=64567456y5474

	
The viewer configuration should be requested by the viewer using the viewerid see [viewer-api documentation](viewer-api.md), ie http://geoviewer/service/viewer/64567456y5474 resulting in [example-viewer-config.json]

Notice the viewer service also provides the option to show all available viewer configurations, allowing for example a IOS viewer to offer the user a choice at startup.

	
##### viewer.config - meta viewer configuration information

The subdirectory *config* of the provided url should contain a file named *viewer.info.js* 

Example:

	http://geoviewer.nl/application/config/viewer.info.js


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
	
##### Tools

A viewer can allow the user to enable and configure specific tools. The administration GUI will provide the user with the means however in order for this to work the viewer should:

- Include a reference to the tool in its viewer.info file.
- Either use an existing tool, or provide the definition of a new tool and request the inclusion of this definition into the geoViewer system.
 
	