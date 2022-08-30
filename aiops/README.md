# Java Client For Nutanix Aiops Versioned APIs

The Java client for Nutanix Aiops Versioned APIs is designed for Java client application developers offering them simple and flexible access to APIs that manage infrastructure on-premises and in the cloud seamlessly through AIOps features such as Analysis, Reporting, Capacity Planning, What if Analysis, VM Rightsizing, Troubleshooting, App Discovery, Broad Observability, and Ops Automation through Playbooks.
## Features
- Invoke Nutanix APIs with a simple interface.
- Handle Authentication seamlessly.
- Reduce boilerplate code implementation.
- Use standard methods for installation.
## Version

- API version: v4.0.a1
- Package version: 4.0.1-alpha-1

## Requirements.

- Maven 3.6
- Java 8

## Usage

### Installation

This library is distributed on [Maven Central](https://mvnrepository.com/repos/central). In order to add it as a dependency, please do the following:

#### Using Maven

```xml
<dependency>
  <groupId>com.nutanix.api</groupId>
  <artifactId>aiops-java-client</artifactId>
  <version>4.0.1-alpha-1</version>
</dependency>
```

#### Using Gradle

```groovy
dependencies {
    implementation("com.nutanix.api:aiops-java-client:4.0.1-alpha-1")
}
```

## Configuration

The Java client for Nutanix Aiops Versioned APIs can be configured with the following parameters

| Parameter | Description                                                                      | Required | Default Value|
|-----------|----------------------------------------------------------------------------------|----------|--------------|
| host      | IPv4/IPv6 address or FQDN of the cluster to which the client will connect to     | Yes      | N/A          |
| port      | Port on the cluster to which the client will connect to                          | No       | 9440         |
| username  | Username to connect to a cluster                                                 | Yes      | N/A          |
| password  | Password to connect to a cluster                                                 | Yes      | N/A          |
| debugging | Runs the client in debug mode if specified                                       | No       | False        |
| verifySsl | Verify SSL certificate of cluster, the client will connect to                    | No       | True         |
| maxRetryAttempts| Maximum number of retry attempts while connecting to the cluster           | No       | 5            |
| retryInterval| Interval in milliseconds at which retry attempts are made                     | No       | 3000         |
| timeout   | Global timeout in milliseconds for all operations                                | No       | 30000        |

### Sample Configuration

```java
import com.nutanix.aio.java.client.ApiClient;

public class Sample {
  public void configureClient() {
    ApiClient client = new ApiClient();
    client.setHost("10.19.50.27"); // IPv4/IPv6 address or FQDN of the cluster
    client.setPort(9440); // Port to which to connect to
    client.setUsername("admin"); // UserName to connect to the cluster
    client.setPassword("password"); // Password to connect to the cluster
  }
}
```

### Authentication
Nutanix APIs currently support HTTP Basic Authentication only, and the client can be configured using the username and password parameters to send Basic headers along with every request.
### Retry Mechanism
The client can be configured to retry requests that fail with the following status codes. The numbers of seconds before which the next retry is attempted is determined by the retryInterval:

- [408 - Request Timeout](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408)
- [502 - Bad Gateway](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502)
- [503 - Service Unavailable](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503)
```java
import com.nutanix.aio.java.client.ApiClient;

public class Sample {
  public void configureClient() {
    ApiClient client = new ApiClient();
    client.setMaxRetryAttempts(5); // Max retry attempts while reconnecting on a loss of connection
    client.setRetryInterval(5000); // Interval in ms to use during retry attempts
  }
}
```

## Usage

### Invoking an operation

```java
// this sample code is not usable directly for real use-case

import com.nutanix.aio.java.client.ApiClient;
import com.nutanix.aio.java.client.api.SampleApi;

public class Sample {
  public void performOperation() {
    ApiClient client = new ApiClient();
    // Configure the client
    // ...
    SampleApi sampleApi = new SampleApi(client);
    final String extId = '66673023168b486898d76bc27e5ce9c2';
    SampleGetResponse sampleResponse = sampleApi.getSampleByExtId(extId);
  }
}
```

### Request Options
The library provides the ability to specify additional options that can be applied directly on the 'ApiClient' object used to make network calls to the API. The library also provides a mechanism to specify operation specific headers.
#### Client headers
The 'ApiClient' can be configured to send additional headers on each request.

```java
import com.nutanix.aio.java.client.ApiClient;

public class Sample {
  public void configureClient() {
    ApiClient client = new ApiClient();
    client.addDefaultHeader("Accept-Encoding","gzip, deflate, br");
  }
}
```
You can also modify the headers sent with each individual operation:

#### Operation specific headers
Nutanix APIs require that concurrent updates are protected using [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) headers. This would mean that the [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) header received in the response of a fetch (GET) operation should be used as an [If-Match](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match) header for the modification (PUT) operation.
```java
import com.nutanix.aio.java.client.ApiClient;


// this sample code is not usable directly for real use-case

public class Sample {
  public void performOperation() {
    ApiClient client = new ApiClient();
    // Configure the client
    // ...
    SampleApi samplesApi = new SampleApi(client);
    final String extId = '66673023168b486898d76bc27e5ce9c2';
    SampleGetResponse sampleResponse = samplesApi.getSampleByExtId(extId);
    // Extract E-Tag Header
    final String eTagHeader = ApiClient.getEtag(sampleResponse);
    // ...
    Sample body = (Sample) sampleResponse.getData();
    HashMap<String, Object> opts = new HashMap<>();
    opts.put("If-Match", eTagHeader);
    samplesApi.updateSampleByExtId(body,extId,opts);
  }
}

```

### List Operations
List Operations for Nutanix APIs support pagination, filtering, sorting and projections. The table below details the parameters that can be used to set the options for pagination etc.

| Parameter | Description
|-----------|----------------------------------------------------------------------------------|
| $page     | specifies the page number of the result set. Must be a positive integer between 0 and the maximum number of pages that are available for that resource. Any number out of this range will be set to its nearest bound. In other words, a page number of less than 0 would be set to 0 and a page number greater than the total available pages would be set to the last page.|
| $limit    | specifies the total number of records returned in the result set. Must be a positive integer between 0 and 100. Any number out of this range will be set to the default maximum number of records, which is 100. |
| $filter   | allows clients to filter a collection of resources. The expression specified with $filter is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response. Expression specified with the $filter must conform to the [OData V4.01 URL](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html#sec_SystemQueryOptionfilter) conventions. |
| $orderby  | allows clients to specify the sort criteria for the returned list of objects. Resources can be sorted in ascending order using asc or descending order using desc. If asc or desc are not specified the resources will be sorted in ascending order by default. For example, 'orderby=templateName desc' would get all templates sorted by templateName in desc order. |
| $select   | allows clients to request a specific set of properties for each entity or complex type. Expression specified with the $select must conform to the OData V4.01 URL conventions. If a $select expression consists of a single select item that is an asterisk (i.e. *), then all properties on the matching resource will be returned. |
| $expand   | allows clients to request related resources when a resource that satisfies a particular request is retrieved. Each expand item is evaluated relative to the entity containing the property being expanded. Other query options can be applied to an expanded property by appending a semicolon-separated list of query options, enclosed in parentheses, to the property name. Allowed system query options are $filter,$select, $orderby. |

List Options can be passed to list operations in order to perform pagination, filtering etc.
```java
import com.nutanix.aio.java.client.ApiClient;
import com.nutanix.aio.java.client.api.ClusterApi;
import com.nutanix.dp1.aio.aiops.v4.clusterMetrics.ClusterListApiResponse;

public class Sample {
  public void performOperation() {
    ApiClient client = new ApiClient();
    // Configure the client
    // ...
    ClusterApi clusterApi = new ClusterApi(client);
    int $page = 0;
    int $limit = 50;
    String $filter = "string_sample_data";
    String $orderby = "string_sample_data";
    ClusterListApiResponse clusterListApiResponse = clusterApi.listResourcesForAllClusters($page, $limit, $filter, $orderby);
  }
}
```

The list of filterable and sortable fields with expansion keys can be found in the documentation [here](https://developers.nutanix.com/).

## API Reference

This library has a full set of [API Reference Documentation](https://developers.nutanix.com/). This documentation is auto-generated, and the location may change.

## License
This library is licensed under Nutanix proprietary license. Full license text is available in [LICENSE](https://developers.nutanix.com/license).

## Contact us
In case of issues please reach out to us at the [mailing list](@sdk@nutanix.com)