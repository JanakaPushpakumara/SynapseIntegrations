# SampleIterateProxy

This is an example of a wso2 EI/MI proxy service configuration in XML that includes a simple Iterate mediator sample.

## Sample request
```
curl --location 'http://localhost:8290/services/SampleIterateProxy' \
--header 'Content-Type: application/json' \
--data '{
   "items": [
      {
         "name": "Item 1",
         "description": "Description for Item 1"
      },
      {
         "name": "Item 2",
         "description": "Description for Item 2"
      },
      {
         "name": "Item 3",
         "description": "Description for Item 3"
      }
   ]
}
```
