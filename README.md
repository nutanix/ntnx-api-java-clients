# Java Client Libraries for Nutanix APIs

This project contains steps  for installing and using Java Client Libraries for Nutanix APIs  grouped together by their namespace. Clients are currently
available for the following namespaces.

| Namespace    | Description                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------------|
| vmm          | Manage the life-cycle of virtual machines hosted on Nutanix clusters.                                        |
| prism        | Manage Tasks, Category Associations, Alerts, Alert policies, Events and Audits.|
| clustermgmt  | Manage Hosts, Clusters, and other Nutanix infrastructure.                                                    |
| aiops        | Manage Nutanix infrastructure using Analysis, Reporting, Capacity Planning, What if Analysis, VM Rightsizing, Troubleshooting, App Discovery, Broad Observability, and Ops Automation through Playbooks.|
| storage      | Manage Volume Groups and Storage Containers hosted on Nutanix clusters.                                     |
| iam          | Manage User Identity and Access.                                                                     |
| lcm          | Manage Infrastructure, Software and Firmware Upgrades. |
| files        | Manage virtual file servers, create and configure shares for client access, protect them using DR and sync policies, provision storage space and administer security controls.|
| networking   | Manage networking configuration on Nutanix clusters, including AHV and advanced networking.|
# Project Structure
Project contains a top level directory corresponding to each namespace as listed above. Each namespace directory contains 
a README with instructions for getting started with that client. 


## Getting Started

The libraries are distributed on [Maven Central](https://mvnrepository.com/repos/central). In order to add it as a dependency, please do the following:

#### Using Maven

In order to use the aiops java client 

```xml
<dependency>
  <groupId>com.nutanix.api</groupId>
  <artifactId>aiops-java-client</artifactId>
  <version>4.0.2-alpha-1</version>
</dependency>
```
and to use the vmm client 

```xml
<dependency>
  <groupId>com.nutanix.api</groupId>
  <artifactId>vmm-java-client</artifactId>
  <version>4.0.2-alpha-1</version>
</dependency>
```
and so on...
#### Using Gradle
In order to use the aiops java client

```groovy
dependencies {
    implementation("com.nutanix.api:aiops-java-client:4.0.1-alpha-1")
}
```

and to use the vmm client

```groovy
dependencies {
    implementation("com.nutanix.api:vmm-java-client:4.0.1-alpha-1")
}
```

and so on...


and consume them as:

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

For detailed instructions on installing individual clients, please refer to the README documentation for the respective clients in the namespace directories.


## Status
These are auto generated Java clients generated from Open API v3.0 yaml specification documents.
Due to the auto-generated nature of these clients, they may contain breaking changes from one release to
the next.

## API Reference
These clients have a full set of [API Reference Documentation](https://developers.nutanix.com/). This documentation is auto-generated, and the location may change.

## License
This library is licensed under Nutanix proprietary license. Full license text is available in [LICENSE](https://developers.nutanix.com/license).

## Contact us
In case of issues please reach out to us at the [mailing list](mailto:sdk@nutanix.com).
