
## Extract data from one CouchDB to another CouchDB instance

### At source CouchDB instance
 
 1. Extract the data from source instance
 ```
 curl -u <couchdb_username>:<couchdb_password> -X GET "http://localhost:5984/db_name/_design/design_document_name/_view/all?include_docs=true" -o output_design_document.json
 ```
 2. Remove the `_rev` field from each document before importing
 ```
 jq "{docs: [ .rows[].doc | del(._rev) ]}" output_design_document.json > new_output_design_document.json
 ```

 3. Copy data from source instance to destination instance using `scp`
 ```
 scp -r /local/path/new_output_design_document.json <remote_username>@<ip>:/destination/folder/path
 ```

### At destination CouchDB instance

4. Import data to destination couchdb instance
```
curl -u <couchdb_username>:<couchdb_password> -X POST "http://localhost:5984/db_name/_bulk_docs" -H "Content-Type: application/json" --data-binary @new_output_design_document.json
```

#### Note :
Please make sure the name `new_output_design_document` (shown here as example) matches the name of the document under a db you want to import data.

