# HowTo map MySQL database to RDF
1.	Connect to database

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img01.jpg)
 

2.	After the connection is established, select and import tables to be mapped from the database.
3.	Import an ontology and/or other vocabulary, schemas that are needed for the mapping.
4.	For each table set up a Base URI: once we want to publish one RDF resource composed from data from different tables then the base URI must be the same for all tables. 

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img02.jpg)
 

5.	Modify id column in each table in order to use it as unique URIs. Click on triangle next to the column name and select PyTransform.  Enter a python expression to change the value in the column:
* one can change value directly in the column or create a new column  to store new value
* my suggestion is to create a new column in case one would like to use the original ids for something else

```python
return "site/"+ getValue("id")
```

This step is needed to generate unique URIs for objects and because the ids in database are unique only within on table, when working with many tables we must specify the context of id (e.g. add name of the table or column header to the URI).

* Click ‘Preview results for top 5 rows’ to check how you python expression worked out.

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img03.jpg)


6.	Workflow:
* Set  Semantic Types for the columns to be serialized to RDF: click on the triangle next to the column name , select ‘Set Semantic Type’– set to ‘uri of Class’ and click edit, then search for the class from imported ontology, click Save. 
* Set relationships among columns: click on the Class of the column you want to connect to (above the table), select ‘Add outgoing link’, and choose the Class you want to relate to, then choose the property (relationship) between these two classes.
* To relate to column containing __literal values__: click on the triangle of literal value column name, set Semantic Type, select ‘Property of Class’, edit, in pop up window set Property and then select which Class this property belongs to, set Literal Type (e.g. xsd:string) and Language (if available).
* To publish data in RDF, go to the table panel (above the table itself), click on triangle next to it, select ‘Publish’ -> RDF, in the field RDF Graphs choose ‘Create New Context’, set the name for the graph (try to stick to convention ‘http://localhost.com/worksheets/your_new_context_name’), select ‘Append’, click ‘Publish’.
The rdf data will be save to OpenRDF repository (click the link in the right upper corner to go there). From the list of repositories choose karma_data, then on the left sidebar choose Contexts, and click on the context name you just created.

To export the rdf file one can also from the table panel in Karma interface, it appear there after selecting Publish – RDF.

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img04.jpg)


* Repeat the same for the next table from database. 
Important here is to set the Foreign Key column to the same Semantic Type this column’s set in original table and set the same URI. 
For example: 
If in first table one sets site_id  URI to semantic type E7_Activity1, then in second table which has site_id as FK it should be assigned the same semantic type E7_Activity1 (note also Activity1). In case one wants to set the semantic type of the same Class but distinguish it from other column (another table column) then one has to set it up to E7_Activity2. 
* To export mapping model, to reapply it again later, go to – Publish - R2RML Model. 

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img05.jpg)


Link ‘R2RML’ model appears on the table panel, click on it and save it to local directory.

![image alt text](https://github.com/zxenia/HowTo/blob/master/images/p01_img06.jpg) 

Next time import the same table from database, then go to Apply R2RML Model, from File , browse the file, choose the saved previously model file and apply it. All previously created mappings should appear on the table. 