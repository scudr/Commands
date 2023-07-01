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