# deployEMDS_EDC
Demo-instructions:

This demo is based on EDC Samples found in https://github.com/eclipse-edc/Samples/tree/main and the documentation found in https://github.com/eclipse-edc/Samples/blob/main/transfer/transfer-00-prerequisites/README.md. It assumes that you have already built the necessary connector (i.e., jar file).

## Configure and start the connectors:
- Open ssh connection to 130.188.160.106
- Navigate to "deployEMDS/Samples/transfer/transfer-00-prerequisites/resources/configuration"
- Edit files "provider-configuration.properties" and "consumer-configuration.properties" E.g., by setting a new value to web.http.management.port=8080
- Start the connectors with the following commands (in the Samples folder):
  - Provider: § java -Dedc.fs.config=transfer/transfer-00-prerequisites/resources/configuration/provider-configuration.properties -jar transfer/transfer-00-prerequisites/connector/build/libs/connector.jar
  - Consumer: § java -Dedc.fs.config=transfer/transfer-00-prerequisites/resources/configuration/consumer-configuration.properties -jar transfer/transfer-00-prerequisites/connector/build/libs/connector.jar

## Data provider - Define data resource to be shared:
- Open Postman and import the included collections. In the provider's collection: 
- Create asset allows you to specify the shared asset. At this point two asset examples are included:
    1. An asset that retrieves data from API without the need of specifying credentials
    2. An asset that uses an API key to retrieve data from an API
- Execute one of the provided asset requests.
- Execute "Create policy" and Create contract" requests. (At this point I cannot find how the contract / policy is linked with the asset?)

## Data consumer: (continue from here and explain the two different consumer options (local and remote))
Start web service that is able to retrieve data from provider
Navigate to "deployEMDS/simpleHTTPserver" folder and start the service with command "python3 dataReceived.py". In this case, the service is deployed on a third server instance ()
In Postman
Execute "Fetch catalog" request
In "Negotiate contract" request paste the policy id of the asset you want to access (found in the response body of the previous request) to policy id field (e.g., "@id": "MS4x:YXNzZXQySWQ=:YTVlYmJjOGUtZGFiNi00Yzg2LWE0NGItZmU1YzBjZTNlZGIw").  Execute "Negotiate contract"
Execute "Get contract agreement id" request. The "{{transfer-process-id}}" parameter is automatically copied from the previous phase.
Execute "Start transfer" command. The "{{contract-agreemeent-id}}" is automatically copied from the previous phase.
Monitor the output of the "simpleHTTPserver" service. You should see the data submitted by the provider in the terminal.
