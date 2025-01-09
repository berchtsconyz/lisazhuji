
# Implement Web Crawling in Amazon Bedrock Knowledge Bases

Amazon Bedrock is a fully managed service that offers access to high-performing foundation models (FMs) from leading AI companies such as AI21 Labs, Anthropic, Cohere, Meta, Stability AI, and Amazon. Through a single API, it provides a comprehensive suite of tools to build secure, private, and responsible generative AI applications.

With Amazon Bedrock, users can experiment with top FMs, customize them with enterprise data using techniques like Retrieval Augmented Generation (RAG), and integrate enterprise systems and data sources. **Amazon Bedrock Knowledge Bases** enables organizations to aggregate data sources into a centralized repository of information, making it easier to build RAG-based applications.

One essential aspect of maintaining accurate and relevant AI applications is accessing up-to-date information from public websites. This post explores how to integrate web crawlers into Amazon Bedrock Knowledge Bases to gather and utilize web data efficiently.

---

## Web Crawler for Knowledge Bases

By adding a web crawler data source to a knowledge base, businesses can build generative AI web applications based on website data. The crawler’s default behavior starts by fetching the provided seed URLs and traversing child links within the same top-level primary domain (TPD) or deeper URL paths.

### Key Considerations for Web Crawling

- URLs must not require authentication.
- URLs should not be IP addresses.
- Only URLs beginning with `http://` or `https://` are supported.
- The crawler supports fetching non-HTML files (e.g., PDFs, text files, CSVs).

Businesses can specify up to 10 source URLs for crawling, which serve as entry points for the crawler. By default, the crawler will not traverse pages across different domains, ensuring relevance and focus.

---

## Understanding Sync Scope

When setting up a knowledge base with web crawl functionality, users can define the sync scope to control which web pages are included. Sync options include:

1. **Default Behavior**: Traverse within the same domain and deeper paths.
2. **Inclusion and Exclusion Filters**: Use regex patterns to refine the URLs to crawl. For example:
   - Exclude `.pdf` files: `^.*\.pdf$`
   - Include URLs containing "products": `.*products.*`

The crawler respects the domain’s `robots.txt` file and adheres to its directives regarding crawl rate and access restrictions.

---

### Simplify Crawling With ScraperAPI

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on analyzing the data. Get structured data from public websites, including census bureaus, property listings, and more.

👉 [**Start your free trial today!**](https://bit.ly/Scraperapi)

---

## Solution Overview

This section outlines the steps to create a knowledge base with a web crawler, test it, and monitor its performance.

### Prerequisites

1. Ensure you have permission to crawl the target URLs and comply with the [Amazon Acceptable Use Policy](https://aws.amazon.com/aup/).
2. Disable bot detection features on the target websites.
3. The crawler uses the user-agent `bedrockbot` for crawling.

---

### Steps to Create a Knowledge Base With a Web Crawler

1. **Navigate to Knowledge Bases**:
   - Go to the Amazon Bedrock console and select **Knowledge bases**.

2. **Create a Knowledge Base**:
   - Click **Create knowledge base** and provide the necessary details, such as name, IAM permissions, and data source.

3. **Configure the Data Source**:
   - Select **Web Crawler** as the data source.
   - Add up to 10 seed URLs.

4. **Select Embedding Model and Vector Database**:
   - Choose **Titan Text Embeddings v2** as the embedding model.
   - Set the vector dimensions to `1024`.
   - Use the **Quick create vector store** option to set up an Amazon OpenSearch Serverless vector search collection.

5. **Finalize and Create**:
   - Review the configurations and click **Create knowledge base**.

---

## Testing the Knowledge Base

Once the knowledge base is created, follow these steps to test it:

1. Navigate to the created knowledge base in the Amazon Bedrock console.
2. Under **Data source**, select the web crawler and click **Sync**. Syncing may take a few minutes to hours depending on the data size.
3. After the sync is complete, use test prompts like:
   - “How do I tour the Seattle Amazon offices?”
   - “Provide me with some information about Amazon’s HQ2.”

The knowledge base will generate responses, including citations to the crawled web pages for verification.

---

## Create a Knowledge Base Using AWS SDK

The following Python code demonstrates how to use the [AWS SDK for Python (Boto3)](https://aws.amazon.com/sdk-for-python/) to create a knowledge base with an embedding model and OpenSearch Service vector collection:

```python
import boto3

client = boto3.client('bedrock-agent')

response = client.create_knowledge_base(
    name='knowledge-base-example',
    roleArn='your-role-arn',
    knowledgeBaseConfiguration={
        'type': 'VECTOR',
        'vectorKnowledgeBaseConfiguration': {
            'embeddingModelArn': 'arn:aws:bedrock:region::foundation-model/amazon.titan-embed-text-v2:0'
        }
    },
    storageConfiguration={
        'type': 'OPENSEARCH_SERVERLESS',
        'opensearchServerlessConfiguration': {
            'collectionArn': 'your-collection-arn',
            'vectorIndexName': 'index_name',
            'fieldMapping': {
                'vectorField': 'documentid',
                'textField': 'data',
                'metadataField': 'metadata'
            }
        }
    }
)
```

---

## Monitoring and Cleanup

### Monitoring
Use [Amazon CloudWatch](https://aws.amazon.com/cloudwatch) logs to monitor crawling activities. Logs show the URLs visited, their retrieval status, and any errors.

### Cleanup
To delete your knowledge base and associated resources:
1. Navigate to **Knowledge bases** in the Bedrock console, delete the selected knowledge base, and note the IAM role and vector database details.
2. Delete the vector database and IAM role through the respective OpenSearch Service and IAM consoles.

---

## Conclusion

Amazon Bedrock Knowledge Bases now support web data sources, enabling businesses to crawl and index public web pages efficiently. This functionality empowers users to build more accurate and effective generative AI applications with up-to-date information. 

To get started with Amazon Bedrock Knowledge Bases, visit the [AWS documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-create.html) or explore [Amazon Bedrock pricing](https://aws.amazon.com/bedrock/pricing/).

👉 [**Try ScraperAPI to enhance your data collection process!**](https://bit.ly/Scraperapi)
