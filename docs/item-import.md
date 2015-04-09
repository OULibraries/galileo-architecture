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
1. Repository Services builds a repository object based on the recipe provided by the mapfile.


### Recipe Files

* TODO: Should these be YAML or JSON? 
* TODO: Do we have to invent this wheel? Or is there something that's already defined that we can just use?

For books, we'd expect a yaml recipe to look something like:

```yaml
import: book
uuid: ba8d9603-14d5-4cb8-8f61-8ddaca9e6be0
metadata:
  marcxml:book.xml 
  pagemeta: page.xml
pages:
  - page2.tiff 00e261a7-8531-4b08-a331-a8730c29e6c2
  - page3.tiff cc8f6120-43a2-4724-a34f-25293f82ea43
  - page4.tiff 8a655bb5-2ff3-4f6c-acef-68baedb87070
  - page5.tiff c553c267-bcba-4ddb-b9bb-0fe45ab2c3aa
```
 
 or in json
 
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
                "page2.tiff",
                "00e261a7-8531-4b08-a331-a8730c29e6c2"
            ],
            [
                "page3.tiff",
                "cc8f6120-43a2-4724-a34f-25293f82ea43"
            ],
            [
                "page4.tiff",
                "8a655bb5-2ff3-4f6c-acef-68baedb87070"
            ],
            [
                "page5.tiff",
                "c553c267-bcba-4ddb-b9bb-0fe45ab2c3aa"
            ]
        ]
    }
}
```
