# Error Codes

All service and device protocols responds with the following error
codes:

## Authorization

Authorization problems are indicated as follows:

  - If the access token specified in the HTTP Authorization header
    either doesn't exist or is invalid:
      - HTTP status 401
  - If the access token is valid but an authorization error occurs while
    processing the message
      - JSON-RPC error code: -32000
          - This can happen in at least the following situation
              - When you try to access another users devices
              - When you haven't authorized ickStream Cloud to access
                one of the backend content services

## Parsing

  - If the JSON-RPC message couldn't be parsed, the service should
    respond with:
      - JSON-RPC eror code: -32600

Please note that in case of a parsing error the response might not
contain a **id** attribute and due to this it might be hard for the
caller to always correlate it to a specific request.

## Invalid methods/parameters

  - If the method in the JSON-RPC request aren't supported, the service
    should respond with:
      - JSON-RPC error code: -32601
  - If the combination of parameters in the JSON-RPC request aren't
    supported, the service should respond with:
      - JSON-RPC error code: -32602

## Other processing errors

  - If an unexpected error occurs in the service when processing the
    request, the service should respond with:
      - JSON-RPC error code: -32001

Generally these kind of errors are returned because of temporary runtime
problems, for example full disk, out of memory or some other unexpected
cause.

[Category:API](Introduction "wikilink")
