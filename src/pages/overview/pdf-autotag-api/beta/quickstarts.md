# Quickstarts
<p>

The AutotagPDF API provides modern cloud-based capabilities for automatically tagging a PDF. The API is accessible through SDKs which help you get up and running quickly. Once you've received your developer credential, download and set up one of the sample projects. After you're familiar with the APIs, leverage the samples in your own server-side code.

</p>

<InlineAlert slots="text"/>

The SDK only supports server-based use cases where credentials are saved securely in a safe environment. SDK credentials should not be sent to untrusted environments or end user devices.



To get started with AutotagPDF API, follow these steps:

- [Getting access](quickstarts/#getting-access)
- [Getting credentials](quickstarts/#getting-credentials)
- [SDK](quickstarts/#sdk)
   - [Java](quickstarts/#java)
      - [Environment setup](quickstarts/#setup-a-java-environment)
      - [Samples project setup](quickstarts/#samples-project-setup)
      - Executing [the samples code](howtos/autotag-api/#code-samples)

## Getting access

Getting access to AutotagPDF API requires an email to be sent to autotag-pdf-api-prerelease-access-request@adobe.com and upon receiving confirmation that your email id has been granted access, proceed to [Getting Credentials](quickstarts/#getting-credentials) workflow.

## Getting credentials

Developing with the AutotagPDF API requires an Adobe-provided credentials. To get one, [click here](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK#), and follow these steps: 
- Select **Get started** option under **Create New Credentials**.
- Login or create an account with the same email id which received access in [Getting Access](quickstarts/#getting-access) step.
- Select **PDF Services API**.
- Fill in the details.
- Uncheck the **Create personalized code sample**.
- Select **Create Credentials**.

The credentials will be downloaded in zip format, which consists of pdfservices-api-credentials.json, private.key and README file. Be sure to copy and save the credentials to a secure location.

## SDK

AutotagPDF API is accessible via SDKs, currently available in Java.

### Java

#### Setup a Java environment

Skip this step, if already done.

1. Install [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Run `javac -version` to verify your install. 
3. Install [Maven](https://maven.apache.org/install.html). 

#### Samples project setup

1. Download the sample project from [here](https://git.corp.adobe.com/dc/dc-cpf-sdk-java-samples/archive/beta.zip).
2. Unzip the downloaded zip file from above step and enter the project directory.
3. Find and replace following files with the one present in the downloaded zip file from [Getting Credentials](quickstarts/#getting-credentials) step:
   1. pdfservices-api-credentials.json
   2. private.key
4. Build the sample project with Maven using this command in terminal: `mvn clean install`. 
5. Test the sample code on the command line or import the samples Maven project into your preferred IDE and run the samples from there. <br />
   **Generate tagged pdf of version 1.7 from a PDF** <br />
   Use the sample below to generate tagged pdf of version 1.7 from a PDF document.  <br />
   Copy this command in terminal to run the sample:
   ```
   mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.pdfservices.operation.samples.autotagpdf.AutotagPDFWithV17
   ``` 
   Click [here](howtos/autotag-api/#code-samples) to view more examples of code samples.
6. The generated output will be downloaded in output directory in samples project.

Here are some key resources:

-   [API reference (Javadoc)](https://git.corp.adobe.com/pages/dc/dc-cpf-sdk-java-samples/apidocs/beta/index.html)
-   [Sample code](https://git.corp.adobe.com/dc/dc-cpf-sdk-java-samples/tree/beta)
-   [Java library](https://artifactory.corp.adobe.com/ui/native/maven-dcapi-release/com/adobe/documentservices/pdfservices-sdk/2.1.0-beta.3).

