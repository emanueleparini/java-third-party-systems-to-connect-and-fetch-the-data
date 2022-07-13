# java-third-party-systems-to-connect-to-and-fetch-the-data

For Creating the software to integrate with a third-party software API, we have to do following steps:

1.	Authentication with the service
2.	Fetch the data.
3.	Parse the data as per the format.
4.	Store the parsed data into respective java Object (POJO).
5.	Writing the data to a database.

## Authentication with service and Fetching data

###### 1.	HTTP Protocol
For HTTP requests, we can use OkHttp open source project. Performance wise it is quite better than Apache HttpClient. 
For making OkHttp connection we need to create OkHttpClient object then we can make a request to a URL through creating a Request object with body and method type. Similarly result can be read and processed through the response object returned after executing the request. 

###### 2.	SFTP Protocol
For connecting through SFTP protocol, we can SSHJ library to download file from the third-party application. 
Initially for connecting via SFTP we will set up SSHClient along with host address, username and password. Then we will create SFTPClient and will download the required file from the server. 

###### 3.	IMAP Protocol
For connecting to IMAP server, we can use JavaMail API to access emails on a remote email server. 
For that we need to create a Session object with default Properties instance along with setting a Store object with IMAP mode, host address, port number, login email address and login password. Then we can access the emails or fetch the required files from the server. 

## Parsing the data format

###### 1.	JSON
For parsing the JSON data format, we have multiple java libraries through which we can do that for example Json-simple, GSON, Jackson and Flexjson. We can choose the library as per our requirement in project. For simplicity we can use GSON or for performance and features we can use Jackson library.

###### 2.	XML
Similarly for XML data parsing, we have DOM parser, SAX parser, StAx reader and JAXB. For multiple features, performance and easy to use we can use SAX parser for the purpose. 

###### 3.	CSV
For CSV, similarly we have many library options to choose from like OpenCSV, Apache Commons csv, super csv, Jackson csv etc.
In my opinion it is better to user OpenCSV as it supports many different reading options and it is quite popular.

## POJO

After parsing the data, we can proceed with creating the java objects for saving the data. For that, we can use POJO (Plain old java objects) which help us to save data into database.


## Writing data to Database

For writing the data to relational database, we can use object – relational mapping tool or framework which is hibernate. It can help us in saving data in object to a relational database. For that, we can annotate our java objects with their right associations and hibernate will handle all tables creation and data insertion to the database. 


## Development Interface

###### HTTP
```
OkHttpClient client = getOkHttpClient();
Request request = createRequest(String url, Map<String, String> parameters);
Response response = client.newCall(request).execute();
```

###### SFTP
```
SSHClient sshClient = setupSshj(String url, String username, String password);
SFTPClient sftpClient = sshClient.newSFTPClient();
sftpClient.get("filetodownload.csv", "file.txt");
sftpClient.close();
sshClient.disconnect();
```

###### IMAP
```
Properties properties = new Properties();
properties.setProperty("mail.imap.ssl.enable", "true");
Session session = javax.mail.Session.getInstance(properties);
Store store = session.getStore("imap");
store.connect("mail.example.com", "mail@example.com", "password");
Folder inbox = store.getFolder("INBOX");
inbox.open(Folder.READ_ONLY);
Message[] messages = inbox.getMessages();
inbox.close(false);
store.close();
```

###### JSON - GSON
```
Gson gson = new Gson();
Reader reader = Files.newBufferedReader(Paths.get("file.json"));
Map<?, ?> map = gson.fromJson(reader, Map.class);
for (Map.Entry<?, ?> entry : map.entrySet()) 
	System.out.println(entry.getKey() + "=" + entry.getValue());
reader.close();
```

###### XML - SAX Parser
```
SAXParserFactory factory = SAXParserFactory.newInstance();
SAXParser saxParser = factory.newSAXParser();
File file = new File("file.xml");
saxParser.parse(file, new ElementHandler());
```

###### CSV - OpenCSV
```
FileReader filereader = new FileReader(file);
CSVReader csvReader = new CSVReader(filereader);
String[] line;
while ((line = csvReader.readNext()) != null) {
	for (String cell : line) 
		System.out.print(cell + "\t");
	System.out.println();
}
```

###### POJO
```
DataObject dataObject = storeInObject(String filePath);
```  

###### Saving to Database - Hibernate
After defining mapping configuration of our database in XML file we can execute following java code. 
```
SessionFactory factory = new Configuration().configure().buildSessionFactory();
Session session = factory.openSession();
Transaction transaction = session.beginTransaction();
transaction = session.beginTransaction();
session.save(dataObject); 
transaction.commit();
session.close();
```
