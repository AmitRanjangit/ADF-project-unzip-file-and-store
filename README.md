# ADF-project-unzip-file-and-store
This project demonstrates how to download a zipped binary file from an HTTP endpoint, unzip it, and store the extracted content in an Azure Blob Storage container using Azure Data Factory (ADF). The pipeline is designed to automate the end-to-end process of fetching external data in compressed format and making it available in raw storage for further processing or analysis.

## Pipeline Name
`pipeline2`

## Objective
To automate the process of:
- Calling an HTTP endpoint to download a `.zip` file
- Decompressing the contents of the zip file using Azure Data Factory
- Storing the uncompressed binary file in Azure Blob Storage

## Activities Used

### 1. Copy Activity (`Copy data1`)
The only activity in the pipeline is a `Copy` activity which performs the entire job of fetching, decompressing, and saving the file.

#### Source Configuration
- **Type**: `BinarySource`
- **Read Settings**: `HttpReadSettings`
  - `requestMethod`: GET
- **Format Settings**: `BinaryReadSettings`
  - **Compression Type**: `ZipDeflateReadSettings`
    - Automatically extracts `.zip` format data during the read process
- **Dataset**: `Binary1`
  - This dataset is configured to use an HTTP-linked service to access the external zipped file.

#### Sink Configuration
- **Type**: `BinarySink`
- **Write Settings**: `AzureBlobStorageWriteSettings`
  - The binary file is written to the configured Azure Blob Storage location
- **Dataset**: `Binary2`
  - This dataset is configured to write data to the target blob container in Azure Storage.

## Concepts and Features Used

### Binary Source and Sink
Used when working with non-structured data such as files, images, videos, or zipped content. In this case, it supports the download and handling of a zipped binary file.

### HTTP Linked Service
The source dataset connects to an HTTP endpoint, allowing ADF to pull data directly from the internet or from a private web service.

### ZipDecompression
ADF supports automatic unzipping of files via the `ZipDeflateReadSettings` configuration in the Binary source's `formatSettings`. This avoids the need for additional compute or external tools to decompress content.

### Azure Blob Storage
ADF writes the unzipped content into an Azure Blob Storage container, enabling further downstream processing or analysis in Azure services like Synapse, Data Lake, or Machine Learning.

## Use Case
This pipeline is particularly useful in scenarios where:
- Data is published in compressed format (e.g., open datasets, nightly file dumps)
- Data needs to be quickly staged in Azure Blob Storage for further use
- Minimal transformation is required during the transfer

## Limitations
- Only works with supported compressed formats like `.zip`
- Does not support dynamic decompression of password-protected files
- The HTTP source must allow direct access (no advanced authentication mechanisms supported out of the box)

## How to Run
1. Set up HTTP and Azure Blob Storage linked services in ADF.
2. Create source and sink datasets (`Binary1`, `Binary2`).
3. Deploy the pipeline.
4. Trigger the pipeline manually or via a scheduled trigger.

## Conclusion
This ADF pipeline effectively automates downloading and extracting zipped binary files from a URL to Azure Blob Storage. It is suitable for simple ETL workflows where zipped external files must be staged for further processing.
