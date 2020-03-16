# Access BigQuery via Jupyter notebooks
1. Download and install Google Cloud SDK Shell [here](https://cloud.google.com/sdk/docs/downloads-interactive#windows)
2. Open Google Cloud SDK shell and type:
```
gcloud auth application-default login
```
3. Log in with your pmi-ops.org account

4. Install the cloud client library (skip this if it's already installed)
```python
pip install --upgrade google-cloud-storage
```
5. Connect to bigquery
```python
from google.cloud import storage

client = storage.Client(project='all-of-us-rdr-prod')
bucket = client.get_bucket('ptsc-fitbit-data-all-of-us-rdr-prod')

# list objects in specified prefix
blobs = bucket.list_blobs(prefix=f'INTRADAY_STEPS/186092')
```



# Access RDR (MySQL) via Jupyter notebooks
1. Download cloud sql proxy [here](https://cloud.google.com/sql/docs/mysql/connect-admin-proxy) and **put it in Desktop**

2. Open a terminal or a command line, and cd to your desktop (or wherever you put the cloud sql)
   
3. copy and paste the following in your command line
```bash
cloud_sql_proxy_x64 -dir=./ -instances=all-of-us-rdr-prod:central1:rdrbackupdb-c=tcp:3308
```
*Note:* Change the port to 3306 or 3309 if 3308 doesn't work

4. Define a configuration file
```yaml
connect_options = {
	'user':'',
	'password':'',
	'host':'127.0.0.1',
	'port':'3308',
    	'database':'rdr',
    	'raise_on_warnings': True,
   }
```
5. Install mysql connector (skip this step if it is already installed)
```python
pip install mysql-connector-python
```

6. Import mysql connector and create a connection object in your notebook
```python
import mysql.connector

con = mysql.connector.connect(**connect_options)
```

7. Write query and use the connection object
With pandas, you can have:
```python
query = "select * from table"

data = pd.read_sql_query(query, con=con)
```
