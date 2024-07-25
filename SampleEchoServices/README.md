# SampleEchoProxy

This is an example of simple Echo Proxy Service configuration in WSO2 ESB (or WSO2 EI) Synapse XML format. 
This proxy service will receive a request and echo it back as a response without any modification. 
The <loopback/> mediator should be used in this context to loop the message back to the out sequence

## Sample request
```
curl --location 'http://localhost:8290/services/SampleEchoProxy' \
--header 'Content-Type: application/json' \
--data '{
   "message": "This is a sample request to the Echo Proxy Service"
}'
```

# SampleEchoAPI
This is an example of simple Echo API Service configuration in WSO2 ESB (or WSO2 EI) Synapse XML format.
This API service will receive a request and echo it back as a response without any modification.
The <respond/> mediator should be used in this context to send the response back to the client.

## Sample request
```
curl --location --request GET 'http://localhost:8290/services/SampleEchoAPI'
```