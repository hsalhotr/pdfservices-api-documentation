# Quickstarts

Before Getting started with Digital Seal API, make sure all the [Prerequisites](prerequisites.md) are met. 

## Workflow

![Seal Workflow](../images/sealFlow.png)

## How-to Guide

### 1. Configure Sealing Parameters

#### Signature Type*
Specifies the type of digital signature being applied
* Author signatures/ CERTIFY : first signature in the document often created by the document author, and there can be at most one in any given document.
* Recipient signatures/ SIGN : Digital signatures which are signed with a certificate.
API currently supports SIGN signature type.

#### Signature Format*
Specifies the format of the digital signature. API supports below formats
* PKCS#7 : Defined in [ISO 32000-1](https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf) section 12.8.3.3 
           PKCS#7 Signatures; the signature value Contents contain a DER-encoded PKCS#7 binary data object.
* PADES : Defined in [ETSI TS 102 778-3](https://www.etsi.org/deliver/etsi_ts/102700_102799/10277803/01.02.01_60/ts_10277803v010201p.pdf) the signature 
            value Contents contain a DER-encoded SignedData object as specified in CMS (Cryptographic Message Syntax). This format is more strict and concrete than PKCS#7.

#### Trust Service Provider's (TSP) Credential Information*
Encapsulates the [certificate credential](/overview/digital-seal-api/prerequisites/#1-procure-certificate-credentials) to be used 
for signing and the associated authentication and authorization data.

<details><summary>Trust Service Provider's(TSP) Credential Information</summary><p>

* ##### TSP Name*
Specifies the name of the Trust Service Provider used to generate the certificate.

* ##### TSP Credential Id*
Specifies the Digital ID stored with the TSP provider that should be used for signing.

* ##### TSP Authorization Context*
Encapsulates the service authorization data required to communicate with the TSP and access CSC provider APIs.

  <details><summary></summary><p>

  ##### Access Token*
  Specifies the service access token used to authorize access to the CSC provider hosted APIs.
   
  ##### Token Type
  Specifies the type of service token which is Bearer.
   
  </p></details>

* ##### TSP Credential Authorization Parameter*
Encapsulates the credentials authorization information required to authorize access to their signing keys.

 <details><summary></summary><p>

 ##### PIN*
 Specifies the PIN associated with credential id.
   
 </p></details>

</p></details>

#### Signature Widget Parameters*
Encapsulates the parameters required to create a new unsigned signature field or sign an existing field.

<details><summary>Signature Widget Parameters</summary><p>

* ##### Field Name
Specified the field name for the signature field.
* ##### Visibility
Specified whether the signature field is visible or not. Set to true to create a visible signature. Set to false to 
create an invisible (hidden) signature. The default value is true.
* ##### Page Number*
Specifies the number of the page to which the signature field should be attached.
* ##### Location*
Encapsulates the parameters related to the location of the signature field.

 <details><summary></summary><p>

  ##### Left*
  Specifies the left-most x-coordinate of the signature appearance's bounding box in default PDF user 
    space units.
  ##### Bottom*
  Specifies the bottom-most y-coordinate of the signature appearance's bounding box in default PDF user 
    space units.
  ##### Right*
  Specifies the right-most x-coordinate of the signature appearance's bounding box in default PDF user 
    space units.
  ##### Top*
  Specifies the top-most y-coordinate of the signature appearance's bounding box in default PDF user 
    space units.
 </p></details>

</p></details>

#### Signature Appearance Parameters*
Encapsulates the parameters related to the appearance of the signature field.

<details><summary>Signature Appearance Parameters</summary><p>

   * ##### Display Parameters*
   An enum set of display items: NAME, DATE, LOGO, DISTINGUISHED_NAME, LABELS. Specifies the information to display in the signature.
   <br/> 
   NAME - Specifies that the signer's name should be displayed in the signature appearance.This is a default value.<br/> 
   DATE - Specifies that the signing date/time should be displayed in the signature appearance. This option only controls whether the value of the 
   time/date in the signature dictionary is displayed or not. This value should not be mistaken for a signed timestamp 
   from a timestamp authority. <br/> 
   DISTINGUISHED_NAME - Specifies that the distinguished name information from the 
   signer's certificate should be displayed in the signature appearance. <br/>
   LABELS - Specifies that text labels should 
   be displayed in the signature appearance. This is a default value. <br/>
   LOGO - Specifies that the logo should be 
   displayed in the signature appearance. If a logo image was not supplied in the request body,  the default Acrobat trefoil image is used. |

</p></details>    

**Example JSON**
```json
{
  "signatureType": "SIGN",
  "signatureFormat": "PADES",
  "cscCredentialInfo": {
    "authorizationContext": {
      "accessToken": "<ACCESS TOKEN>",
      "tokenType": "Bearer"
    },
    "credentialAuthParameters": {
      "pin": "<PIN>"
    },
    "providerName": "<PROVIDER_NAME>",
    "credentialId": "<CREDENTIAL_ID>"
  },
  "signatureFieldOptions": {
    "pageNumber": 1,
    "fieldName": "Signature",
    "visible": true,
    "location": {
      "top": 300,
      "bottom": 250,
      "left": 300,
      "right": 500
    }
  },
  "signatureAppearanceOptions": {
    "displayOptions": [
      "DATE",
      "LOGO",
      "DISTINGUISHED_NAME"
    ]
  }
}
```

### 2. Generate sealed PDF using Digital Seal API
Digital Seal API is invoked after interacting with third-party dependencies, configuring sealing parameters and collating 
required input data to form the request. The request-id generated in the response headers can be polled anytime 
to retrieve the output sealed PDF. <br/>
Upload all the input assets to get the corresponding [Asset Ids](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=2589901901#CustomTempstorage(forDCPlatformAPIs)-Upload/downloadassets). 
Given below are the request body parameters:

#### Document Asset Id*
A file asset ID of an input PDF document in which seal has to be applied

#### Logo Asset Id
A file asset ID of the logo/watermark/background image to use as part of the 
signature field's signed appearance. The format of the asset should be among 
the below format.

1. application/pdf
2. image/jpeg
3. image/png

#### Signature Asset Id
A file asset ID of the signature image (i.e., drawn signature) to use as part of 
the signature field's signed appearance. The format of the asset should be among 
the below format.

1. application/pdf
2. image/jpeg
3. image/png

#### Sealing Parameters*
A collection of [parameters](/overview/digital-seal-api/quickstarts/#1-configure-sealing-parameters) 
that will be applied during sealing creation

There are two ways to access Digital Seal API

**2.1 REST API**
You can use our cloud based REST API to generate seal on PDF documents.
<InlineAlert slots="text"/>

Before you begin with the REST API, refer [How To Get Started](https://documentcloud.adobe.com/document-services/index.html#how-to-get-started-) to learn more about generating the required credentials and invoking the APIs.

```javascript
curl --location --request POST 'https://cpf-stage-ue1.adobe.io/ops/:create?respondWith=%7B%22reltype%22%3A%20%22http%3A%2F%2Fns.adobe.com%2Frel%2Fprimary%22%2C%20%22fragment%22%3A%20%22output%3DdocumentOut%22%7D' \
--header 'Authorization: Bearer {{Placeholder for token}}' \
--header 'Accept: application/json, text/plain, */*' \
--header 'x-api-key: {{Placeholder for client_id}}' \
--header 'Prefer: respond-async,wait=0' \
--form 'contentAnalyzerRequests="{
   \"cpf:engine\":{
      \"repo:assetId\":\"urn:aaid:cpf:Service-2629a6ba5aba43eca5ec4c65e1ceaed0\"
   },
   \"cpf:inputs\":{
      \"documentIn\":{
         \"dc:format\":\"application/pdf\",
         \"cpf:location\":\"InputFile\"
      },
      \"logoIn\":{
        \"dc:format\":\"image/png\",
        \"cpf:location\":\"LogoFile\"
      },
      \"signatureIn\":{
        \"dc:format\":\"image/png\",
        \"cpf:location\":\"SignatureFile\"
      },
      \"params\":{
         \"cpf:inline\":{
            \"sealingOptions\":{
               \"signatureType\":\"SIGN\",
               \"signatureFormat\":\"PADES\",
               \"cscCredentialInfo\":{
                  \"authorizationContext\":{
                     \"accessToken\":\"<ACCESS TOKEN>\",
                     \"tokenType\":\"Bearer\"
                  },
                  \"credentialAuthParameters\":{
                     \"pin\":\"<PIN>\"
                  },
                  \"providerName\":\"<PROVIDER_NAME>\",
                  \"credentialId\":\"<CREDENTIAL_ID>\"
               },
               \"signatureFieldOptions\":{
                  \"pageNumber\":1,
                  \"fieldName\":\"Signature\",
                  \"visible\":true,
                  \"location\":{
                     \"top\":300,
                     \"bottom\":250,
                     \"left\":300,
                     \"right\":500
                  }
               },
               \"signatureAppearanceOptions\":{
                  \"displayOptions\":[
                     \"DATE\",
                     \"LOGO\",
                     \"DISTINGUISHED_NAME\"
                  ]
               }
            }
         }
      }
   },
   \"cpf:outputs\":{
      \"documentOut\":{
         \"dc:format\":\"application/pdf\",
         \"cpf:location\":\"file\"
      }
   }
}"' \
--form 'InputFile=@"{{Placeholder for the input PDF document (absolute path)}}"' \
--form 'LogoFile=@"{{Placeholder for LOGO Image (absolute path)}}"' \
--form 'SignatureFile=@"{{Placeholder for Signature Image (absolute path)}}"'

```

**2.2 PDF Services JAVA SDK**
Alternatively, you can use our offering through [PDF Services SDK](../pdf-services-api#sdk).

<InlineAlert slots="text"/>

To get started with PDF Services SDK, refer [Quickstarts](../pdf-services-api).

```javascript
public class DigitalSealSuccessTest {

    public void digitalSealSuccessTest(String id) throws Exception {
        TestCase testCase = testCases.getTestCaseById(id);
        if(Utils.shouldRunInSuite(testCase)) {
            testCase.printConfig();
            ExecutionContext executioncontext = Utils.getExecutionContext(testCase);
            //Get the input document to perform the sealing operation
            FileRef sourceFile = Utils.addDigitalSealSourceFileInfo(testCase);

            //Get the background logo for signature , if required.
            FileRef logoFile = Utils.addDigitalSealLogoFileInfo(testCase);
            //Get the drawn signature image, if required.
            FileRef signFile = Utils.addDigitalSealSignFileInfo(testCase);

            //Create SignatureDisplayOptions and add the required signature display items to it
            SignatureDisplayOptions signatureDisplayOptions = new SignatureDisplayOptions();
            signatureDisplayOptions.addDisplayItem(SignatureDisplayItem.DATE);
            signatureDisplayOptions.addDisplayItem(SignatureDisplayItem.LOGO);

            //Set the Signature Field Name to be created in input PDF document.
            String signFieldName = "Signature";
            //Set the page number in input document for applying signature.
            int signPageNumber = 1;
            //Set if signature should be visible or invisible.
            boolean signVisible = true;
            //Create SignatureLocation instance and set the coordinates for applying signature
            SignatureLocation signatureLocation = new SignatureLocation(150, 250, 350, 200);;
            //Create SignatureFieldOptions instance with required details.
            SignatureFieldOptions signatureFieldOptions = new SignatureFieldOptions.Builder(signatureLocation, signPageNumber)
                    .setFieldName(signFieldName)
                    .setVisible(signVisible)
                    .build();

            //Set the name of CSC Provider being used.
            String providerName = "intesi";
            //Set the access token to be used to access CSC provider hosted APIs.
            String accessToken = "0a07948d-bfb7-4a77-8b07-199e0e9a2208";
            //Set the credential ID.
            String credentialID = "[ADOBE_TEST]_AUTO_12834_SIGN_1647837397700:44";
            //Set the PIN generated while creating credentials.
            String credentialPin = "12345678";

            CSCCredentialOptions signatureCredentialOptions = SignatureCredentialOptions.cscCredentialOptions(providerName, credentialID, credentialPin, accessToken);

            //TODO: usage of signAuth is temporary and will be removed after service to service token support.
            String signAuth = "3AAABLZtkPdAGKvHEl8kAGdhMI0QPHM7TNZIoe3Mf6wGk_2p6wwFoDx6AdEAU1KTdZCFxBBCu_B2GYBFWUGDJQlphrkWsmbFO";
            SignatureOptions signatureOptions = new SignatureOptions(SignatureType.SIGN, SignatureFormat.PKCS7, signatureCredentialOptions,
                    signatureFieldOptions, signatureDisplayOptions, signAuth);

            //Build a DigitalSealOptions instance using the SignatureOptions instance
            DigitalSealOptions digitalSealOptions = new DigitalSealOptions(signatureOptions);

            //Build the DigitalSealOperation instance using the digitalSealOptions instance
            DigitalSealOperation digitalSealOperation = DigitalSealOperation.createNew(digitalSealOptions);

            //Set the input source file for digitalSealOpertaion instance
            digitalSealOperation.setInput(sourceFile);

            //Set the optional input logo image for digitalSealOpertaion instance
            digitalSealOperation.setLogoImage(logoFile);

            //Set the optiona input signature image for digitalSealOpertaion instance
            digitalSealOperation.setSignatureImage(signFile);

            //Execute the operation
            FileRef result = digitalSealOperation.execute(executioncontext);
        }
    }
}
```




