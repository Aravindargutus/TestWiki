# Catalyst Java SDK — Overview

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/overview/
> Captured: 2026-04-05

Java SDK for Catalyst paves the way for creating microservices, interactive web and mobile applications with Java programming elements and associating them with Catalyst components. The SDK offers the necessary structures to access the Catalyst APIs comfortably. It acts as a wrapper for the REST APIs and helps you use Catalyst services effectively.

Catalyst currently supports the following versions of Java:
- Java 8
- Java 11
- Java 17

## Initialize Project

To initialize Catalyst project, add the code snippet below to your Java source code as the very first statement, before you start writing your business logic.

```java
ZCProject.initProject();
```

**Note:** In Java SDK it is not mandatory to include this initialize command as it will be automatically initialized in functions.

### Initialize Catalyst Projects with Specific SDK Scopes

Catalyst allows you to initialize a project using the following scopes:

- **Admin**: You have unrestricted access to all the components and their respective functionalities. For example, you have complete access to the Data Store to perform all operations like Read, Write, Delete, etc.
- **User**: You can restrict access to components, and specific functionalities. For example, you can provide Read access alone to Data Store.

**Notes:**
- It is not mandatory for you to initialize the projects with scopes. By default, a project that is initialized will have Admin privileges.
- Ensure you have initialized the Catalyst SDK with the appropriate scope while you engineer your business logic. The permissions you define for your scope control your end-user's actions.
- Scopes only apply to operations related to Data Store, File Store, and ZCQL.
- Depending on how you engineer your business logic, you can decide if your end-users can perform Admin or User actions. This is decided based on the role assigned to your end-user when they sign up to your application in Catalyst Authentication. The permissions for the roles can be configured in the Scopes & Permissions section of the Data Store and File Store.

**Initialize with Admin Scope:**
```java
ZCProject adminProject = ZCProject.initProject("admin", ZCUserScope.ADMIN);
ZCQL.getInstance(adminProject).executeQuery("select * from test");
```

**Initialize with User Scope:**
```java
ZCProject userProject = ZCProject.initProject("user", ZCUserScope.USER);
ZCQL.getInstance().executeQuery("select * from test");
```

## Class Hierarchy

All the Catalyst components are modelled as Java classes with their members and methods defining the behaviour of the component.

- **ZCProject** is the fundamental base class of the SDK package. It has methods to initialize the catalyst project configurations and associate the components of the project.
- The class relations and hierarchy of the SDK follow the project hierarchy in Catalyst.
- Each class has functions to fetch its properties and to fetch the data of its immediate child entities through an API call. For example, a Catalyst Data Store class, ZCDataStore will have member functions to access tables that can use the functions of its immediate child class ZCTable to set the table name, ID, etc.

## Instance Objects

It is not always effective to follow the class hierarchy all the way from the top to fetch the data of a component at a lower level, since this would involve API calls at every level. In order to avoid this, every component class has a `getInstance()` method to get its dummy object and methods to get dummy objects of its child entities.

**Note:** `getInstance()` methods will not have any of their properties filled in since no API call will be made. This will just return a dummy object that will be only used to access the non-static methods of the class.

To retrieve the properties of a Catalyst component, call the component's object with its `getInstance()` method, then use the same object to call the other methods defined by the component. This avoids unnecessary API calls.

## Instance Objects

## Exceptions

Unexpected faulty behaviours are called exceptions. All errors and exceptions are handled by a class called `ZCException` defined by the Java SDK. There are `ZCServerException` and `ZCClientException` classes to catch the specific exceptions thrown by the client and server codes.

---

## Upgrade Java SDK

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/upgrade-sdk/

Catalyst constantly endeavours to provide you with the latest, most relevant, and secure SDK packages.

### Steps to Upgrade

Two methods to upgrade:

1. **Download from console**: Go to Catalyst console → Profile icon → Download SDKs → Java icon. Unzip and paste contents in the `lib` folder of your Java function.
2. **Update through Maven**: Update your Maven configurations if you use Maven for Java development.

**Note:** You need to paste the SDK content in the `lib` folder of every Java function you have created and initialized in your project.

---

## Integrate SDK in Third-Party Apps

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/integrate-sdk-in-third-party-apps/

You can integrate and use the Catalyst Java SDK methods in applications deployed outside the Catalyst environment. For example:
- A React app hosted on Vercel using a Flask backend can upload documents to Catalyst Cloud Scale Stratus
- A data pipeline running on AWS EC2 can push customer data into Catalyst Cloud Scale Data Store using ZCQL queries

### Prerequisites

- **Project ID**: Unique identifier of your Catalyst project (found in Settings → Project Settings → General)
- **ZAID (Zoho Account ID)**: Unique portal identifier assigned by Catalyst to link your project with the Catalyst environment (development or production)
- **Environment**: Target environment (development or production)
- **OAuth Credentials**: Client ID, Client Secret, Refresh Token — obtained via Catalyst's self-client portal (API console at api-console.zoho.com)

### Integration Code Snippet (SpringBoot Example)

```java
package com.example.demoapp;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.stereotype.Component;
import java.util.List;
import java.util.logging.Logger;
import org.json.simple.JSONObject;
import com.zc.common.ZCProject;
import com.zc.common.ZCProjectConfig;
import com.catalyst.config.ZCThreadLocal;
import com.zc.api.APIConstants.ZCAuthType;
import com.zc.api.APIConstants.ZCUserScope;
import com.zc.auth.ZCAuth;
import com.zc.component.USER_TYPE;
import com.zc.component.object.ZCObject;
import com.zc.component.object.ZCRowObject;
import com.zc.component.object.ZCTable;

@SpringBootApplication
public class DemoappApplication {
    private static final Logger logger = Logger.getLogger(DemoappApplication.class.getName());

    public static void main(String[] args) {
        SpringApplication.run(DemoappApplication.class, args);
    }

    @Component
    public static class DataProcessor implements CommandLineRunner {
        @Override
        public void run(String... args) {
            try {
                ZCThreadLocal.putValue("user_type", USER_TYPE.ADMIN);
                JSONObject oAuthParams = new JSONObject();
                oAuthParams.put("client_id", CLIENT_ID);
                oAuthParams.put("client_secret", CLIENT_SECRET);
                oAuthParams.put("refresh_token", REFRESH_TOKEN);
                oAuthParams.put("grant_type", "refresh_token");
                ZCAuth auth = ZCAuth.getInstance(oAuthParams);
                auth.setScope(ZCUserScope.ADMIN);
                ZCProjectConfig config = ZCProjectConfig.newBuilder()
                    .setProjectId(PROJECT_ID)
                    .setProjectKey(ZAID)
                    .setZcAuth(auth)
                    .setProjectDomain("https://api.catalyst.zoho.com")
                    .setEnvironment("Development")
                    .build();
                ZCProject project = ZCProject.initProject(config, "");
                ZCStratus stratus = ZCStratus.getInstance(project);
                List buckets = stratus.listBuckets();
            } catch (Exception e) {
                logger.severe("Error during data processing: " + e.getMessage());
            }
        }
    }
}
```

### SDK Operations Categories

The SDK provides operations across these categories:
- **General**: Project-level operations
- **Serverless**: Functions, cron, etc.
- **Cloud Scale**: Data Store, File Store, ZCQL, Stratus, Authentication
- **Zia Services**: AI/ML services
- **SmartBrowz**: Browser automation
- **Job Scheduling**: Scheduled tasks
- **Pipelines**: Data pipelines
- **QuickML**: Machine learning
- **Connectors**: Third-party integrations
