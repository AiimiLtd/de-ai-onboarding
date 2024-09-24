# Mermaid Diagrams

Mermaid is a tool for creating diagrams in markdown. It is useful for creating flowcharts, sequence diagrams, gantt charts, and more. The diagrams are created using a simple syntax that is easy to learn and use.

## Free and Open Source

Mermaid is a free and open source tool that is available on [GitHub](https://github.com/mermaid-js/mermaid-live-editor) it also has an easy to use web front end at [Mermaid.live](https://mermaid.live). This makes it easy to get started with Mermaid and create diagrams quickly.

## Use Cases

### ETL

Lets say you are building a data warehouse for your client and you want to plan out the ETL process. You can use Mermaid to create a flowchart that shows the steps involved in the process. This can help you visualize the process and identify any potential issues or bottlenecks.

### Entity Relationship Diagram (ERD)

You can also use Mermaid to create an Entity Relationship Diagram (ERD) for your database. This can help you visualize the relationships between the tables in your database and identify any potential issues or optimizations.

### Downstream / Upstream dependencies

You can use Mermaid to create a flowchart that shows the dependencies between different tasks or processes. This can help you identify any dependencies that need to be resolved before a task can be completed.

## Syntax

The syntax for creating diagrams in Mermaid is simple and easy to learn. Here are some examples of the syntax for different types of diagrams:

### Flowchart

``` text
flowchart TD  
    A[Christmas] -->|Get money| B(Go shopping)  
    B --> C{Let me think}  
    C -->|One| D[Laptop]  
    C -->|Two| E[iPhone]  
    C -->|Three| F[fa:fa-car Car]  
```

``` mermaid
flowchart TD
    A[Christmas] -->|Get money| B(Go shopping)
    B --> C{Let me think}
    C -->|One| D[Laptop]
    C -->|Two| E[iPhone]
    C -->|Three| F[fa:fa-car Car]
```

### ERD

``` text
erDiagram  
    CAR ||--o{ NAMED-DRIVER : allows  
    CAR {  
        string registrationNumber PK  
        string make  
        string model  
        string[] parts  
    }  
    PERSON ||--o{ NAMED-DRIVER : is  
    PERSON {  
        string driversLicense PK "The license #"  
        string(99) firstName "Only 99 characters are allowed"  
        string lastName  
        string phone UK  
        int age  
    }  
    NAMED-DRIVER {  
        string carRegistrationNumber PK, FK  
        string driverLicence PK, FK  
    }  
    MANUFACTURER only one to zero or more CAR : makes  
```

``` mermaid
erDiagram  
    CAR ||--o{ NAMED-DRIVER : allows  
    CAR {  
        string registrationNumber PK  
        string make  
        string model  
        string[] parts  
    }  
    PERSON ||--o{ NAMED-DRIVER : is  
    PERSON {  
        string driversLicense PK "The license #"  
        string(99) firstName "Only 99 characters are allowed"  
        string lastName  
        string phone UK  
        int age  
    }  
    NAMED-DRIVER {  
        string carRegistrationNumber PK, FK  
        string driverLicence PK, FK  
    }  
    MANUFACTURER only one to zero or more CAR : makes  
```

