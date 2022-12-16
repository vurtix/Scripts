 Here's a Python script that uses the Splunk Enterprise SDK to extract data from a Splunk index, perform a transformation on the data, and write the results to a file:
 ```python
 import splunklib.client as client
import json
import time

# Connect to Splunk
service = client.connect(
    host="splunk_host",
    port=8089,
    username="splunk_username",
    password="splunk_password"
)

# Run a Splunk search
search_query = 'search index=myindex | top 10 sourcetype'
job = service.jobs.create(search_query)

# Wait for the search to complete
while not job.is_done():
    time.sleep(1)

# Get the search results
results = job.results()

# Perform a transformation on the results
transformed_results = []
for result in results:
    transformed_result = {
        'source': result['sourcetype'],
        'count': int(result['count'])
    }
    transformed_results.append(transformed_result)

# Write the transformed results to a file
with open('transformed_results.json', 'w') as f:
    json.dump(transformed_results, f)
```
This script does the following:

1. Imports the necessary modules: the Splunk Enterprise SDK, the json module for working with JSON data, and the time module for adding delays.
2. Connects to Splunk using the client.connect() function from the Splunk Enterprise SDK. You will need to specify the hostname, port number, and login credentials for your Splunk instance.
3. Runs a search using the service.jobs.create() function from the Splunk Enterprise SDK. The search query is specified as a string, and it can be any valid Splunk search query. In this example, the query searches the "myindex" index and returns the top 10 sourcetypes.
4. Waits for the search to complete using a while loop and the job.is_done() function from the Splunk Enterprise SDK. This is important because the search results are not immediately available after the search is submitted.
5. Retrieves the search results using the job.results() function from the Splunk Enterprise SDK. This returns the results as a Python iterator, which allows you to iterate through the results one by one.
6. Performs a transformation on the results using a for loop. In this example, the transformation creates a new dictionary for each result with the sourcetype as the "source" key and the count as the "count" key. The transformed results are stored in a list called "transformed_results".
7. Writes the transformed results to a file using the json module's json.dump() function. The file is opened using the open() function, and it is passed to json.dump() as an argument. The file is automatically closed when the with block ends.
