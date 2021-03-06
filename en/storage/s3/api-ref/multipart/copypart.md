# copyPart method

Copies part of an object.

It has the same functionality as [[!TITLE]](uploadpart.md), but data is not passed in the request body, it is copied from an existing object.

## Request {#request}

```
PUT /{bucket}/{key}?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
```

### Path parameters {#path-parameters}

| Parameter | Description |
| ----- | ----- |
| `bucket` | Name of the resulting bucket. |
| `key` | Key of the resulting object. ID under which the object is saved in [!KEYREF objstorage-name]. |

### Query parameters {#request-parameters}

| Parameter | Description |
| ----- | ----- |
| `partNubmer` | ID that you assigned to the uploaded part. |
| `uploadId` | ID of the multipart upload returned by [!KEYREF objstorage-name] at the [start](startupload.md). |

### Headers {#request-headers}

In a request, use the necessary [common request headers](../common-request-headers.md).

The `Content-Length` header is required. The headers listed in the table below are also required.

| Header name | Description |
| ----- | ----- |
| `x-amz-copy-source` | The name of the bucket and the key of the object whose data will be copied, separated by the `/` character.<br/><br/>For example, `x-amz-copy-source: /source_bucket/sourceObject`. |
| `x-amz-copy-source-range` | Byte range to copy from the source object. For example, if you specify `x-amz-copy-source-range:bytes=10-36`, then [!KEYREF objstorage-name] will copy the range from the 10th to the 36th bytes of the source object. |

If you want to add copy conditions, use the headers listed in the table below.

Use the headers from the table below if you need to change the default behavior of the `copy` method.

| Header name | Description |
| ----- | ----- |
| `x-amz-copy-source-if-match` | Condition for copying an object.<br/><br/>If the object's `ETag` matches the one specified in the header, the object is copied.<br/><br/>If the condition is not met, [!KEYREF objstorage-name] returns error 412.<br/><br/>Can be used with the `x-amz-copy-source-if-unmodified-since` header. |
| `x-amz-copy-source-if-none-match` | Condition for copying an object.<br/><br/>If the object's `ETag` does not match the one specified in the header, the object is copied.<br/><br/>If the condition is not met, [!KEYREF objstorage-name] returns error 412.<br/><br/>Can be used with the  `x-amz-copy-source-if-modified-since` header. |
| `x-amz-copy-source-if-unmodified-since` | Condition for copying an object.<br/><br/>The object is copied if it has not been modified since the specified time.<br/><br/>If the condition is not met, [!KEYREF objstorage-name] returns error 412.<br/><br/>Can be used with the `x-amz-copy-source-if-match` header. |
| `x-amz-copy-source-if-modified-since` | Condition for copying an object.<br/><br/>The object is copied if it has been modified since the specified time.<br/><br/>If the condition is not met, [!KEYREF objstorage-name] returns error 412.<br/><br/>Can be used with the `x-amz-copy-source-if-none-match` header. |

## Response {#response}

### Headers {#response-headers}

A response can only contain [common response headers](../common-response-headers.md).

### Response codes {#response-codes}

For a list of possible responses, see [[!TITLE]](../response-codes.md).

Additionally, [!KEYREF objstorage-name] may return errors described in the table below.

| Error | Description | HTTP code |
| ----- | ----- | ----- |
| `NoSuchUpload` | The specified upload does not exist. The specified upload ID might be incorrect or the upload was completed or deleted. | 404 Not Found |
| `EntityTooSmall` | The part is too small.<br/><br/>The uploaded part must be at least 5 MB. | 400 Bad Request |

### Data schema {#response-scheme}

```
<CopyObjectResult>
   <LastModified>2019-02-15T14:32:00</LastModified>
   <ETag>"9bgh7535f2734ec974343yuc93985328"</ETag>
</CopyObjectResult>
```

| Element | Description |
| ----- | ----- |
| `CopyObjectResult` | Contains response elements.<br/><br/>Path: `/CopyObjectResult`. |
| `ETag` | `ETag` of the resulting part of multipart upload.<br/><br/>Path: `/CopyObjectResult/ETag`. |
| `LastModified` | Date when a part of multipart upload was last modified.<br/><br/>Path: `/CopyObjectResult/LastModified`. |

