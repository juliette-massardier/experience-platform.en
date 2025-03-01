---
keywords: Experience Platform;home;popular topics;query service;query templates;api guide;templates;Query service;
solution: Experience Platform
title: Query Templates API Endpoint
topic-legacy: query templates
description: The following documentation walks through the various API calls you can make using query templates for the Query Service API.
exl-id: 14cd7907-73d2-478f-8992-da3bdf08eacc
---
# Query templates endpoint

## Sample API calls

Now that you understand what headers to use, you are ready to begin making calls to the [!DNL Query Service] API. The following sections walk through the various API calls you can make using the [!DNL Query Service] API. Each call includes the general API format, a sample request showing required headers, and a sample response.

### Retrieve a list of query templates

You can retrieve a list of all query templates for your IMS Organization by making a GET request to the `/query-templates` endpoint. 

**API format**

```http
GET /query-templates
GET /query-templates?{QUERY_PARAMETERS}
```

| Property | Description |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | (*Optional*) Parameters added to the request path which configure the results returned in the response. Multiple parameters can be included, separated by ampersands (`&`). The available parameters are listed below. |

**Query parameters**

The following is a list of available query parameters for listing query templates. All of these parameters are optional. Making a call to this endpoint with no parameters will retrieve all query templates available for your organization.

| Parameter | Description |
| --------- | ----------- |
| `orderby` | Specifies the field by which to order results. The supported fields are `created` and `updated`. For example, `orderby=created` will sort results by created in ascending order. Adding a `-` before created (`orderby=-created`) will sort items by created in descending order. | 
| `limit` | Specifies the page size limit to control the number of results that are included in a page. (*Default value: 20*) |
| `start` | Offsets the response list, using zero-based numbering. For example, `start=2` will return a list starting from the third listed query. (*Default value: 0*) |
| `property` | Filter results based on fields. The filters **must** be HTML escaped. Commas are used to combine multiple sets of filters. The supported fields are `name` and `userId`. The only supported operator is `==` (equal to). For example, `name==my_template` will return all query templates with the name `my_template`. |

**Request**

The following request retrieves the latest query template created for your IMS organization.

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns HTTP status 200 with a list of query templates for the specified IMS Organization. The following response returns the latest query template created for your IMS organization.

```json
{
    "templates": [
        {
            "sql": "SELECT *\nFROM\n  accounts\nLIMIT 10\n",
            "name": "Test",
            "id": "f7cb5155-29da-4b95-8131-8c5deadfbe7f",
            "updated": "2019-11-21T21:50:01.469Z",
            "userId": "{USER_ID}",
            "created": "2019-11-21T21:50:01.469Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "DELETE"
                },
                "update": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "PUT",
                    "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
                }
            }
        }
    ],
    "_page": {
        "orderby": "-created",
        "start": "2019-11-21T21:50:01.469Z",
        "next": "2019-11-21T21:50:01.469Z",
        "count": 1
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates?orderby=-created&start=2019-11-21T21:50:01.469Z"
        },
        "prev": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates?orderby=-created&start=2019-11-21T21:50:01.469Z&isPrevLink=true"
        }
    },
    "version": 1
}
```

>[!NOTE]
>
>You can use the value of `_links.delete` to [delete your query template](#delete-a-specified-query-template).

### Create a query template

You can create a query template by making a POST request to the `/query-templates` endpoint.

**API format**

```http
POST /query-templates
```

**Request**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/query-templates
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "sql": "SELECT * FROM accounts;",
        "name": "Sample query template"
    }'
```

| Property | Description |
| -------- | ----------- |
| `sql` | The SQL query you want to create. |
| `name` | The name of the query template. |

**Response**

A successful response returns HTTP status 202 (Accepted) with details of your newly created query template.

```json
{
    "sql": "SELECT * FROM accounts;",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:20:09.670Z",
    "userId": "{USER_ID}",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>You can use the value of `_links.delete` to [delete your query template](#delete-a-specified-query-template).

### Retrieve a specified query template

You can retrieve a specific query template by making a GET request to the `/query-templates/{TEMPLATE_ID}` endpoint and providing the ID of the query template in the request path.

**API format**

```http
GET /query-templates/{TEMPLATE_ID}
```

| Property | Description |
| -------- | ----------- | 
| `{TEMPLATE_ID}` | The `id` value of the query template you want to retrieve. |

**Request**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns HTTP status 200 with details of your specified query template.

```json
{
    "sql": "SELECT * FROM accounts;",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:20:09.670Z",
    "userId": "A5A562D15E1645480A495CE1@techacct.adobe.com",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>You can use the value of `_links.delete` to [delete your query template](#delete-a-specified-query-template).

### Update a specified query template

You can update a specific query template by making a PUT request to the `/query-templates/{TEMPLATE_ID}` endpoint and providing the ID of the query template in the request path.

**API format**

```http
PUT /query-templates/{TEMPLATE_ID}
```

| Property | Description |
| -------- | ----------- |
| `{TEMPLATE_ID}` | The `id` value of the query template you want to retrieve. |

**Request**

>[!NOTE]
>
>The PUT request requires both the sql and name field to be filled, and will **overwrite** the current content of that query template.

```shell
curl -X PUT https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "sql": "SELECT * FROM accounts LIMIT 20;",
    "name": "Sample query template"
 }'
```

| Property | Description |
| -------- | ----------- |
| `sql` | The SQL query you want to update. |
| `name` | The name of the scheduled query. |

**Response**

A successful response returns HTTP status 202 (Accepted) with the updated information for your specified query template.

```json
{
    "sql": "SELECT * FROM accounts LIMIT 20;",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:29:20.028Z",
    "lastUpdatedBy": "{USER_ID}",
    "userId": "{USER_ID}",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>You can use the value of `_links.delete` to [delete your query template](#delete-a-specified-query-template).

### Delete a specified query template

You can delete a specific query template by making a DELETE request to the `/query-templates/{TEMPLATE_ID}` and providing the ID of the query template in the request path.

**API format**

```http
DELETE /query-templates/{TEMPLATE_ID}
```

| Property | Description |
| -------- | ----------- |
| `{TEMPLATE_ID}` | The `id` value of the query template you want to retrieve. |

**Request**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response** 

A successful response returns HTTP status 202 (Accepted) with the following message. 

```json
{
    "message": "Deleted",
    "statusCode": 202
}
```
