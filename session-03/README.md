# 03. Database Backend

Goal: Start using a NoSQL data store in applications
To that end, do as much as you can of the following:


## Hands-on

1. Review Google’s data store options
    - Use this guide: https://cloud.google.com/storage-options/
2. Follow the Google Cloud Datastore example [here*](hands-on-01).
3. Revisit the Shopping List from [Week 4](../session-02)
    - Store the JSON data in a data store
    - Use Google’s Cloud Datastore: https://cloud.google.com/datastore/
    - You will need to decide on suitable Entity Groups.
3. Add images to the database
    - Store links in the JSON documents
    - Store the images in a separate data store
    - Use Google’s Cloud Storage: https://cloud.google.com/storage/

   **Note: You can have a look at the [Google Bookshelf
   tutorial](https://cloud.google.com/go/getting-started/using-cloud-storage),
   which follows a similar strategy on how to split metadata and binary data.**


*Based on Francesc's [go-web-workshop](https://github.com/campoy/go-web-workshop).
