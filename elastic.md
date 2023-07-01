## Instructions for Elasticsearch Directory

1. **Accessing Elasticsearch:**
    - Navigate to the elastic directory and run `elasticsearch.bat` using the following path:
        ```
        bin\elasticsearch.bat
        ```

2. **Resetting Password:**
    - To reset the password, use the `elasticsearch-reset-password.bat` script in the bin directory with the `-u` parameter followed by 'elastic':
        ```
        bin\elasticsearch-reset-password.bat -u elastic
        ```

## Elasticsearch Commands

- **Cluster Health:**
    - To get the health status of your cluster, use the following GET request:
        ```
        GET /_cluster/health
        ```

- **Node Information:**
    - To view information about nodes in your cluster, use the following GET request:
        ```
        GET /_cat/nodes?v
        ```

- **Indices Information:**
    - To view all indices in your cluster, including those from wildcards, use the following GET request:
        ```
        GET /_cat/indices?v&expand_wildcards=all
        ```