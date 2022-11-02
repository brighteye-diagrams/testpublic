## Introduction

The Knowledge module in Momentum centralizes various information about products and processes from different data sources. Knowledge can be information of various types as documents, images, videos, binary, integer data, etc. 

## Scope 

First of all, Knowledge serves as an aggregating entity of data about a specific object – work center, transport unit, equipment, delivery lot, cargo, etc. In fact, knowledge is a custom property of an object containing references to heterogeneous documentation. It provides operator information about recipes, instructions on how to work with some equipment and in general any information that helps in work in various forms, for example, pictures `(*.jpg, *.png)`, text documents `(*.txt, *.doc, *.docx, *.pdf)`, video `(*.mp4)` or even videos from `YouTube`.
Another scope of Knowledge is to provide an opportunity to extend the existing data model of a specific object(s) in Runtime trough Momentum Manager. Thus, Knowledges are additional custom properties of specific object(s) used in case when the existing set of properties defined by data model is not sufficiently describe the object(s). Such properties can be used in business logic development as any object properties. 

## External Knowledge 

Starting from version 17.1.0, it became possible to store knowledges such as documents, pictures, video's, instructions, etc. not only in the internal Momentum database, but also in external sources such as file shares or SharePoint Online. Each property of External Knowledge allows to define reference to information stored on external data source that can be displayed in Momentum Web Clients and Momentum Manager.

Momentum supports connection to the following external sources:
- `Connection to a content website` – data stored in global network and can be accessed via HTTP(s);
- `Connection to a local file storage` – data stored on local network machines;
- [`Connection to a SharePoint Online site`](https://dev.azure.com/brighteye-documentation/Developers%20documentation/_wiki/wikis/Articles/82/SharePoint-online-configuration?anchor=**1.-overview-of-steps-to-perform**) – data stored on SharePoint Online site.

Starting from version 17.3.0, it became possible to directly access the external knowledge from the browser – "client access" mode, which gives better performance for big files. This option can be activated by setting “Web browser direct access” property of knowledge. 
Depending on the type of file and external source, the process of obtaining data from external knowledge may differ. The table below represents methods of loading external knowledge files that Momentum uses depending on the type of files (columns) and data sources (rows).

![KnowledgeModes](https://github.com/brighteye-diagrams/Test/blob/main/KnowledgeModesPIC.png)

## "Method A". Set URL directly to DOM element

> **Prerequisites:**
“Web browser direct access” property in Momentum is activated. **YouTube link is an exception. YouTube video will be loading trough “A” way independently on value of “browser access” property.**

![ExternalKnowledgesA](https://github.com/brighteye-diagrams/Test/blob/main/ExternalKnowledgesA.svg)

To display external knowledge by the web client, the web server requests the knowledge properties from the Momentum server. The web server analyzes the file type of the external knowledge, depending on it forms the structure of the web page and sets the URL knowledge property as the attribute value of the corresponding DOM element of the HTML page, for example, `<img src=”URL property”/>`. If authentication is required to access the file, as in the case of SharePoint, the web server generates a new pre-authenticated link to the file and sets it as the value of the source attribute. Since the knowledge link is defined as a property of the basic html tag, the browser automatically loads the file.

> **Advantages of method "A":
If connection to file sources is fast and stable, this option will be the best and fastest.**

## "Method B". Download content via JavaScript

> **Prerequisites:**
“Web browser direct access” property in Momentum is activated. 

![ExternalKnowledgesB](https://github.com/brighteye-diagrams/Test/blob/main/ExternalKnowledgesB.svg)

To display external knowledge by the web client, the web server requests the knowledge properties from the Momentum server. The web server analyzes the file type of external knowledge, and depending on it forms the general structure of the web page. During the generation of a web page, the web server calls the JS method FETCH, which is executed by the browser and whose argument is the URL knowledge property. As a result of executing the FETCH URL method, the browser downloads the knowledge file from an external data source and displays it on the web client page.
Since method "B" in the current version is used to download pdf files, the open library pdf.js is used to display them on the web page.

> **Advantages of method "B":
If connection to file sources is fast and stable, this option will be the best and fastest for displaying PDF documents.**

## "Method C". Download content by Momentum Server + server side cache + client side cache

> **Prerequisites:**
Specific file type and external source.

![ExternalKnowledgesC](https://github.com/brighteye-diagrams/Test/blob/main/ExternalKnowledgesC.svg)

To display external knowledge by the web client, the web server requests knowledge properties from the Momentum server. The web server analyzes the file type of external knowledge, and depending on it forms the general structure of the web page. During the generation of a web page, the web server sends a request to the Momentum server to download the knowledge file, if the type of external source is not local file storage. The Momentum server downloads data to its cache using the URL property link and transfers them to the web server. The web server stores data in its cache and ensures that the file is displayed on the web page. If the type of external source is local file storage Momentum server transfer the file directly to web server.

> **Advantages of method "C":
Due to caching in Momentum Server and Momentum Web Server, this will be the best choice when the connection speed to the sources where the files are stored is low.**

