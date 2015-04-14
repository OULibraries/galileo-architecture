# The File Archive


The File archive will do 
* BagIt
* Files
* Folders

But specifically not do more finely grained content modeling. 

## Browsing Content
1. The archive will have an enhanced web directory with thumbnails

## Access Control
1. Access will be restricted to internal users and systems.  The repository will be the public-facing interface for this content, and it will be allowed read-access to the archive.

## File Archive Bags
1. In general, bags will correspond to the files required to load a collection or compound object like a book. For example, a bag for an islandora book object contains all the original, high-resolution TIFF image files.
1. Each bag will contain metadata files for each TIFF file. For example, a book bag may contain metadata files for each page image.
1. If a bag contains multiple TIFF files, these files have alphanumerically ordered names to generated the pages of books in a correct order. 
1. The islandora object metadata file (e.g., islandora book object) is also included in the bags.


## Glacier
1. Valid bags in the file archive get uploaded to Amazon Glacier for DR purposes. Archive revisions can be restored with date (not time) granularity.
