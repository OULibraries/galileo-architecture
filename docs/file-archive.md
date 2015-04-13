# The File Archive


The File archive will do 
* BagIt
* Files
* Folders

But specifically no content modeling. 


## File Archive Bags:
1. In general, bags will correspond to the files required to load a collection or compound object like a book. For example, a bag for an islandora book object contains all the original, high-resolution TIFF image files.
1. Each bag also contains an XML metadata file that contains all the metadata information for the TIFF file(s). For example, a book bag may contain the metadata file page.xml for the TIFF files. 
1. If a bag contains multiple TIFF files, these files have alphanumerically ordered names to generated the pages of books in a correct order. 
1. The islandora object metadata file (e.g., islandora book object) is also included in the bags.
