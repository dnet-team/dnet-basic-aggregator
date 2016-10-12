# Creating a new datasource

The GUI for the addition of a new datasource is available and you are warmly invited to use it instead of the old method (method 2) described below.
## Method 1: Registering a data source via the D-Net GUI
Click on the menu "DataSource Management" -> "Add new Datasource" and fill the form. The "add" button at the bottom will be enabled when all the required fields are filled correctly.
- **Datasource ID**: mandatory. The identifier of the data source. The value must be compliant to the regular expression ```^\w{12}::\w+\$```: 12 ASCI characters (```[A-Za-z0-9_]```), followed by ```::```, followed by any ASCI characters. Example: ```12Chars_____::theId```.
- **Official Name**: mandatory. The official name of the data source.
- **English Name**: optional. The name of the data source in English. If not present, the value provided in **Official Name** is automatically used.
- **Organization**: mandatory. The name of the organization in charge of the data source.
- **Typology class**: mandatory. The type of the data source. Select the proper value from the drop-down list.
- **WebSite Url**: mandatory. The URL of the web site of the data source or its organization.
- **Contact Email**: mandatory. The email address of the person that administers the data source (e.g. the repository manager).
- **Country**: mandatory. The location of the data source or jurisdiction of its organization. Select the proper country code from the drop-down list. This value is used to show the proper flag in the DataSource API Management page.
- **Latitude** and **Longitude**: optional. Geographical coordinates of the data source. Those values used to be used for the data source visualization in a map. The map feature has been temporarly disabled since release 1.2.0.
- **Namespace Prefix**: mandatory. Records collected from this data source are assigned an internal dnet identifier that starts with this value. It must be composed of 12 ASCI characters (```[A-Za-z0-9_]```).
- **Software Typology**: optional. Software used for the implementation of the data source. Example: DSpace, ePrints, OCTOPUS, CRIS.
- **Logo Url**: optional. Url to the logo image of the data source.

Click on the "add" button to register the data source, or click "reset" to clear all fields in the form.

Now that you have a data source, you can add "APIs". An "API" is an Application Programming Interface that can be used to collect data and metadata from the data source.
- Click on the menu "DataSource Management" -> "Add new API" 
- Select the typology of the data source you want to add the API to
- Choose the data source from the list
- Fill the form for the creation of a new API. The "add" button at the bottom will be enabled when all the required fields are filled correctly.
	- **Api ID**: mandatory. Any ASCI characters (```[A-Za-z0-9_]```).
	- **Compatibility level**: mandatory. Select from the drop-down list. Available values are:
		- native: use this compatibiity for API from which you can collect metadata records
		- files: use this compatibility for API from which you can download digital files (e.g. PDF)
		- UNKNOWN
	- **Content description**: mandatory. Select from the drop-down list. Available values are:
		- file::PDF: select for API with compatibility "files"
		- metadata: select for API with compatibility "native"
	- **Protocol**: mandatory. Select the protocol used by the API from the drop-down list. Most probably you'll need to choose one of:
		- _ftp2_: the data source offer an FTP/SFTP API
		- _oai_: the data source offer an OAI Publisher
		- _httpCSV_: the data source expose a CSV/TSV file over HTTP/HTTPS
	- Based on the protocol choice, you'll be asked to fill specific fields. 
	- **BaseURL**: mandatory. 
	- **Xpath for Metadata Identifier**: mandatory. The element of the XMLs collected from the API that must be used to generate the internal dnet identifier of the records. In typical cases of OAI API, you want to fill this field with ```//*[local-name()='header']/*[local-name()='identifier']```. 
 
Click on the "add" button to add the API, or click "reset" to clear all fields in the form.
 
To delete an API, please go to its DataSource API Management page and click on the "Delete API" red button on the top left. Please note that the button is visible only if the API has no workflows.
If the API has workflows, please delete all of them and then you will be able to delete the API.

To work with data sources and APIs, please follow the instruction in the README.md file, section "Using D-Net".


## Method 2 -- the old method
This method requires you to modify by hand the D-Net XML profiles. You are warmly invited to use method 1 described above.

- Locate the D-Net profile of the mock datasource: http://localhost:8280/app/mvc/ui/isManager.do#/profile/a9b3496c-2ce7-43e7-83a1-f934b22a04d0_UmVwb3NpdG9yeVNlcnZpY2VSZXNvdXJjZXMvUmVwb3NpdG9yeVNlcnZpY2VSZXNvdXJjZVR5cGU=
- Click on the “Edit” button and copy all the content of the profile
- Click on the “Abort” button
- Click on “Register new profile” on the left side menu
- Paste the content copied in 2 in the dedicate text box. You can resize the box to make it larger at your convenience
- Change the following values in the XML you copied:
	- Datasource identifier: <code>DATASOURCE_ORIGINAL_ID</code> (current value is TEST, please use a string with no spaces)
	- Datasource official name: <code>OFFICIAL_NAME</code> (the official name of the datasource)
	- Datasource english name: <code>ENGLISH_NAME</code> (the english name of the datasource)
	- Institution: <code>REPOSITORY_INSTITUTION</code> (the institution responsible for the datasource)
	- Interface identifier, access protocol and url
	- Datasource namespace prefix: 12 chars namespace that will be used to generate the identifiers of aggregated records. Ensure that the value of NamespacePrefix is EXACTLY 12 chars.

Example for the interface params: 
```
<INTERFACE id="api_________::minimal::mock::metadata" Please change the “mock” substring with something else.
<ACCESS_PROTOCOL format="oai_dc" set="">oai</ACCESS_PROTOCOL>
<BASE_URL>http://theOAIBaseURL</BASE_URL>
```
Please add the “set” parameter (attribute of `ACCESS_PROTOCOL`), if needed, and properly change the OAI-PMH base URL you want to harvest from.

Example for the NamespacePrefix: 
```
<FIELD>
	<key>NamespacePrefix</key>
	<value>dnet_____min</value>
</FIELD>
```

Finally, you can click on “Register” and you’ll find the new datasource together with the others.
	
#Need support?
Do not hesitate to contact dnet-team@isti.cnr.it

