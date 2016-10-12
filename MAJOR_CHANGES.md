# Major changes from 1.1.1 to 1.2.0

- D-Net enabling services:
	- using cache for subscription access
	- support only one subscription registry
- Mongo based services (mdstore, oaistore, wf logging):
	- using API of mongo-java-driver 3.2.2, removed usage of deprecated methods
	- tracking the number of stored records to possibly highlight the collection of records with the same identifier
- GUI:
	- enabling deletion of APIs via GUI
    - enabling editing of metadata_identifier_path
    - more info available in the datasource section
    - removed map of data sources (TODO: adapt to the new google map API)
- Metadata collection:
	-  handling HTML illegal entities in collected XMLs
- Indexing:
	- default query operator for "bag of words" queries set to AND instead of OR
- OAI Publisher:
    - fixed cache management
    - fixed oai consistency (post feed) workflow branch