# Creating a new datasource

The GUI for the addition of a new datasource is under implementation.
Meanwhile, you can manually create a new datasource by working directly on the D-Net XML profiles.
Just follow these steps and do not worry: it is faster to do it then to write about it!

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

