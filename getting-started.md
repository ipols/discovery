---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}

# Getting started with the API

In this short tutorial, we introduce the {{site.data.keyword.discoveryshort}} API and go through the process of creating a private data collection and searching it.
{: shortdesc}

## Before you begin
{: #before-you-begin}

- Create an instance of the service:
    - {: download} If you're seeing this, you created your service instance. Now get your credentials.
    - Create a project from a service:
        1.  Go to the {{site.data.keyword.watson}} Developer Console [Services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/developer/watson/services){: new_window} page.
        1.  Select {{site.data.keyword.discoveryshort}}, click **Add Services**, and either sign up for a free {{site.data.keyword.Bluemix_notm}} account or log in.
        1.  Type `discovery-tutorial` as the project name and click **Create Project**.
- Copy the credentials to authenticate to your service instance:
    - {: download} From the service dashboard (what you're looking at):
        1.  Click the **Service credentials** tab.
        1.  Click **View credentials** under **Actions**.
        1.  Copy the `username`, `password`, and `url` values.
        {: download}
    - From your **discovery-tutorial** project in the Developer Console, copy the `username`,  `password`, and `url` values for `"discovery"` from the  **Credentials** section.

<!-- Remove this text after dedicated instances have the Developer Console: begin -->

If you use {{site.data.keyword.Bluemix_dedicated_notm}}, create your service instance from the [{{site.data.keyword.discoveryshort}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/catalog/services/discovery/){: new_window} page in the Catalog. For details about how to find your service credentials, see [Service credentials for Watson services ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually){: new_window}.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Step 1: Create an environment
{: #create-an-environment}

In a bash shell or equivalent environment such as Cygwin, use the `POST /v1/environments` method to create an environment. Think of an environment as the warehouse where you are storing all your boxes of documents.

1.  Issue the following command to create an environment that is called `my-first-environment`. Replace `{username}` and `{password}` with the service credentials you copied earlier:

    ```bash
    curl -X POST -u "{username}":"{password}" -H "Content-Type: application/json" -d '{ "name":"my-first-environment", "description":"exploring environments"}' "api/v1/environments?version=2017-11-07"
    ```
    {: pre}

    The API returns information such as your environment ID, environment status, and how much storage your environment is using.

1.  Check the environnment status periodically until you see a status of `ready`.
    - Issue a call to the `GET /v1/environments/{environment_id}` method to retrieve the status of your environment. Replace `{username}`, `{password}`, and `{environment_id}` with your information:

    ```bash
    curl -u "{username}":"{password}" https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}?version=2017-11-07
    ```
    {: pre}

    The status must be `ready` before you can create a collection.

## Step 2: Create a collection
{: #create-a-collection}

Now that the environment is ready, you can create a collection. Think of a collection as a box where you will store your documents in your environment.

1.  You need the ID of your default configuration first. To find your default `configuration_id`, use the `GET /v1/environments/{environment_id}/configurations` method. Replace `{username}`, `{password}`, and `{environment_id}` with your information:

    ```bash
      curl -u "{username}":"{password}" https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}/configurations?version=2017-11-07
    ```
    {: pre}
1.  Use the `POST /v1/environments/{environment_id}/collections` method to create a collection called **my-first-collection**. Replace `{username}`, `{password}`, `{environment_id}` and `{configuration_id}` with your information:

    ```bash
    curl -X POST -u "{username}":"{password}" -H "Content-Type: application/json" -d '{"name": "my-first-collection", "description": "exploring collections", "configuration_id":"{configuration_id}" , "language": "en_us"}' https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}/collections?version=2017-11-07
    ```
    {: pre}

    The API returns information such as your collection ID, collection status, and how much storage your collection is using.
1.  Check the collection status periodically until you see a status of `online`.
    - Issue a call to the `GET /v1/environments/{environment_id}/collections/{collection_id}` method to retrieve the status of your collection.  Again, replace `{username}`, `{password}`, `{environment_id}` and `{configuration_id}` with your information:

    ```bash
    curl -u "{username}":"{password}" https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}/collections/{collection_id}?version=2017-11-07
    ```
    {: pre}

## Step 3: Download the sample documents
{: #download-sample-documents}

Download these sample documents: <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc1.html" download>test-doc1.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc2.html" download>test-doc2.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc3.html" download>test-doc3.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, and <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc4.html" download>test-doc4.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.

## Step 4: Upload the documents
{: #upload-the-documents}

1.  Now, add the example documents to your collection. This example uploads the document **test-doc1.html** to your collection. Replace `{username}`, `{password}`, `{environment_id}` and `{configuration_id}` with your information. Modify the location of the sample document to point to where you saved the `test-doc1.html` file.
    - Use the `POST /v1/environments/{environment_id}/collections/{collection_id}/documents` method:

        ```bash
        curl -X POST -u "{username}":"{password}" -F "file=@test-doc1.html" https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}/collections/{collection_id}/documents?version=2017-11-07
        ```
        {: pre}

    Alternatively, use one of the SDKs listed in the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/watson/developercloud/discovery/api/v1/){: new_window}:
    - Java:

      ```java
      Discovery discovery = new Discovery("2017-11-07");
      discovery.setEndPoint("https://gateway.watsonplatform.net/discovery/api/v1");
      discovery.setUsernameAndPassword("{username}", "{password}");
      String environmentId = "{environment_id}";
      String collectionId = "{collection_id}";
      String documentJson = "{\"field\":\"value\"}";
      InputStream documentStream = new ByteArrayInputStream(documentJson.getBytes());

      CreateDocumentRequest.Builder builder = new CreateDocumentRequest.Builder(environmentId, collectionId);
      builder.inputStream(documentStream, HttpMediaType.APPLICATION_JSON);
      CreateDocumentResponse createResponse = discovery.createDocument(builder.build()).execute();
      ```
      {: codeblock}

    - Python:

      ```python
      import sys
      import os
      import json
      from watson_developer_cloud import DiscoveryV1

      discovery = DiscoveryV1(
        username="{username}",
        password="{password}",
        version="2017-11-07"
      )

      with open((os.path.join(os.getcwd(), '{path_element}', '{filename}' as fileinfo:
        add_doc = discovery.add_document('{environment_id}', '{collection_id}', file_info=fileinfo)
      print(json.dumps(add_doc, indent=2))
      ```
      {: codeblock}

    - Node.js:

      ```javascript
      var DiscoveryV1 = require('watson-developer-cloud/discovery/v1');
      var fs = require('fs');

      var discovery = new DiscoveryV1({
        username: '{username}',
        password: '{password}',
        version_date: '2017-11-07'
      });

      var file = fs.readFileSync('{/path/to/file}');

      discovery.addDocument(('{environment_id}', '{collection_id}', file),
      function(error, data) {
        console.log(JSON.stringify(data, null, 2));
        }
      );
      ```
      {: codeblock}

1.  Repeat this process for each of the other 3 sample files.

## Step 5: Query your collection
{: #query-your-collection}

Finally, use the `GET /v1/environments/{environment_id}/collections/{collection_id}/query` method to search your collection of documents.

The following example returns all entities that are called **IBM**. Replace `{username}`, `{password}`, `{environment_id}` and `{configuration_id}` with your information:

```bash
curl -u "{username}":"{password}" 'https://gateway.watsonplatform.net/discovery/api/v1/environments/{environment_id}/collections/{collection_id}*/query?version=2017-11-07&query=enriched_text.entities.text:IBM'
```
{: pre}

## Next steps
{: #next-steps}

You have successfully queried documents in the environment and collection you created. You can now begin customizing your collection by adding more documents and enrichments, and customizing conversion settings. For more information about the API, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/watson/developercloud/discovery/api/v1/){: new_window}.
