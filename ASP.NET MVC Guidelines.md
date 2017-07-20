# ASP.NET MVC Guidelines #

## Database First ##
When using a database first approach, the database and their respective tables are created first, and the model will be generated according to it. 

Not the best approach to do a iterative development of the application model, because it's harder to generate a new Entity Model from the Relational Model, because Entity Framekwork decides to create a new connection do the database, so the DbContext and connection needs to be changed.

To connect to any SQL Server Express in localhost, the only thing that changes is the data source, that change from version to version, in 2016 is (local)\SQLEXPRESS. The other options really depends on the programmer preferences, also the connectionString property is filled automatically by EntityFramework the first time the model is generated.

Localhost example:
```xml
<connectionStrings>
  <add name="yourDatabaseConnectionName" 
  connectionString="metadata=res://*/Models.EntityModel.MeetingEduDBEntities.csdl|res://*/Models.EntityModel.MeetingEduDBEntities.ssdl|res://*/Models.EntityModel.MeetingEduDBEntities.msl;
  provider=System.Data.SqlClient;
  provider connection string=&quot;
  data source=(local)\SQLEXPRESS;
  initial catalog=yourDatabaseName;
  AttachDbFilename=yourDatabasePath;
  integrated security=True;
  MultipleActiveResultSets=True;
  App=EntityFramework&quot;" 
  providerName="System.Data.EntityClient" />
</connectionStrings>
```

In production or when using an hosted database the procedure is pretty much the same, depending on the service provider chosen, due to the database version and if they allow remote connections.

## Code First ##

## Controllers ##

## Models ##

## Views ##
To define Views in ASP.NET MVC it's used a templating language called Razor, this way the views will not be decoupled from the server, leading to a higher relation(dependability) between Client and Server.

## ViewModels ##

## Authentication ##
Use Custom Authentication, with the use of FormsAuthentication that sets a cookie

## Deploy to a Windows Web Server ##

#### Using Web Deploy ####

#### Files and Folders Exclusions ####

#### Pages loading slowly? ####
That's due to the Roslyn compiler.
