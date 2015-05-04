# Building Repository Objects from the File Archive



We're initially interested in just two diferent content types:

1. books
1. large image collections

from the File Archive. 

## Workflow for Importing a Book 

Here's our general plan for how importing a book should work. Currently
ignoring the existence of Amazon Glacier, and the desire to changelog the
bags.

1. DigiLab scans a book to produce a set of tiffs and notes page-level descriptive metadata
1. DigiLab bags tiffs for export to file archive
1. File Archive extracts page level metadata
1. File Archive rebags with page level metadata
1. Repository Services is given a bag path and catalog id and then:
  * exports metadata from the catalog and exhibit site to the bag path
  * generates UUIDs for the book and for each page of the book
  * generates an import recipe file that includes those UUIDs
1. File Archive rebags with the files generated in previous step
1. Repository Services builds a repository object based on the recipe provided by the mapfile, accessing bag contents from the File Archive via http


### Sample Recipe

For books, we'd expect a JSON recipe to look something like:

 
 ```json
 {
    "recipe": {
        "import": "book",
        "uuid": "ba8d9603-14d5-4cb8-8f61-8ddaca9e6be0",
        "metadata": {
            "marcxml": "book.xml",
            "pagemeta": "page.xml"
        },
        "pages": [
            {
                file: "page1.tiff",
                uuid: "00e261a7-8531-4b08-a331-a8730c29e6c2",
                sha1: "69df526c83c9b0fbfae0a4c6618873be"
            },
            [
                file: "page2.tiff",
                uuid: "cc8f6120-43a2-4724-a34f-25293f82ea43",
                sha1: "69df526c83c9b0fbfae0a4c6618873be"
                
            ],
            [
                "page3.tiff",
                "8a655bb5-2ff3-4f6c-acef-68baedb87070",
                "69df526c83c9b0fbfae0a4c6618873be"
            ],
            [
                "page4.tiff",
                "c553c267-bcba-4ddb-b9bb-0fe45ab2c3aa",
                "69df526c83c9b0fbfae0a4c6618873be"
            ]
        ]
    }
}
```

## Object Creation Details

We currently conceive object creation as a command line process where the interface would look something like
```
drush import-function-name $file-archive-recipe-uri
```
We may also want to specify a parent for the object to be created, but that could also be part of the item recipe. 

### Initial requirements for import
1. The importing logic has to be applicable or easily extensible to other islandora content models for future use.
1. The importing process has to have an exception handling system, reporting specified errors and preventing system from collapse or other misconducts.
1. The importing should be a transactional process, which means the importing can never be partially complete. Already-imported files will be cleaned up if the import fails.
1. After each conducted import, a report should be generated.
1.  In the rare cases of updating/deleting Islandora content object pages, we can add/change/delete page images and the page numbers are also updated correspondly (Maybe implemented in the future).


### Import Report:

 

1. Import success or not.
2. Import completion date and time.
3. If the import fails:
  * Reasons for failure;
  * List of the files already imported, each of which should be identified by its file name and the UUID, if applicable, used in the bag, together with the generated Islandora ID.
  * In addition, an indication whether the imported files have been cleaned up (deleted from the Islandora box).
4. If the import succeeds:
  * New Islandora object information.
  * List of the imported files, accompanied by the information of original file name, UUID (if applicable), and the generated Islandora ID.
  * Results of running the checksum verificaiton.

### Sample Report

```
Import time: 04-09-2015, 5:30pm
New Islandora book object islandora:131 has been successfully generated from Bag 5678 (UUID 679dsfhg-sdfds), with 6 pages.
page 1, file: page1.TIFF; ID: islandora:00e261a7-8531-4b08-a331-a8730c29e6c2;
page 2, file: page2.TIFF; ID: islandora:cc8f6120-43a2-4724-a34f-25293f82ea43;
page 3, file: page3.TIFF; ID: islandora:cc8f6120-43a2-4724-a34f-25293f82ea43;
page 4, file: page4.TIFF; ID: islandora:8a655bb5-2ff3-4f6c-acef-68baedb87070";
page 5, file: page5.TIFF; ID: islandora:c553c267-bcba-4ddb-b9bb-0fe45ab2c3aa
page 6, file: page6.TIFF; UUID: hdskd8485-dsfhl; islandora ID: islandora:137;

page 1: successful;
page 2: successful;
page 3: successful;
page 4: successful;
page 5: successful;
page 6: successful;
Import Complete: 04-09-2015, 7:30pm
```
