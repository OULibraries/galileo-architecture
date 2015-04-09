# Building Repository Objects from the File Archive



## Example: Importing a book

Here's our general plan for how importing a book should work. Currently
ignoring the existence of Amazon Glacier, and the desire to changelog the
bags.

1. DigiLab scans a book to produce a set of tiffs and notes page-level descriptive metadata
1. DigiLab bags tiffs for export to file archive
1. File Archive extracts page level metadata to xml
1. File Archive rebags with page level metadata
1. Repository Services is given a bag path and catalog id and then:
  * exports metadata from the catalog and exhibit site to the bag path
  * generates UUIDs for the book and for each page of the book
  * generates an import recipe file that includes those UUIDs
1. File Archive rebags with the files generated in previous step
1. Repository Services builds a repository object based on the recipe provided by the mapfile, accessing bag contents from the File Archive via http


### Recipe Files

For books, we'd expect a JSON recipe to look something like:

 
 ```json
 {
    "receipe": {
        "import": "book",
        "uuid": "ba8d9603-14d5-4cb8-8f61-8ddaca9e6be0",
        "metadata": {
            "marcxml": "book.xml",
            "pagemeta": "page.xml"
        },
        "pages": [
            [
                "page1.tiff",
                "00e261a7-8531-4b08-a331-a8730c29e6c2"
            ],
            [
                "page2.tiff",
                "cc8f6120-43a2-4724-a34f-25293f82ea43"
            ],
            [
                "page3.tiff",
                "8a655bb5-2ff3-4f6c-acef-68baedb87070"
            ],
            [
                "page4.tiff",
                "c553c267-bcba-4ddb-b9bb-0fe45ab2c3aa"
            ]
        ]
    }
}
```

# Bagit Import Project Specifics
## Islandora content models for bagit import project:
1. book
2. large image
 
## The bag data structure:
1. Each of the bags is for building for an individual islandora content object and is identified by a unique bag folder name. For example, a bag for an islandora book object contains all the original, high-resolution TIFF image files.<br><br> 
1. Each bag also contains an XML metadata file that contains all the metadata information for the TIFF file(s). For example, a book bag may contain the metadata file page.xml for the TIFF files. <br><br>
1. If a bag contains multiple TIFF files, these files have alphanumerically ordered names to generated the pages of books in a correct order. <br><br>
1. The islandora object metadata file (e.g., islandora book object) is also included in the bags.
 
## The command line importing process:
1. The importing logic has to be applicable or easily extendable to other islandora content models for future use.<br><br>
1. The importing process has to have correct exception handling system, reporting specified errors and preventing system from collapse or other misconducts.<br><br>
1. The format of command line should look like this:
<br>&nbsp;&nbsp;&nbsp;&nbsp;<b>drush import-function-name $metadata-uri $bag-uri<b><br><br>
1. The importing should be a transactional process, which means the importing can never be partially complete. The already imported files will be cleaned up if the import fails.<br><br>
1. After each conducted import, there should be a report generated.<br><br>
2. In the rare cases of updating/deleting islandora content object pages, we can add/change/delete page images and the page numbers are also updated correspondly (Maybe implemented in the future).

## Sample importing report:

 
