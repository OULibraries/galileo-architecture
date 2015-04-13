# The File Archive


The File archive will do 
* BagIt
* Files
* Folders

But specifically no content modeling. 


## File Archive Bags:
1. In general, bags will correspond to the files required to load a collection or compound object like a book. For example, a bag for an islandora book object contains all the original, high-resolution TIFF image files.
1. Each bag also contains metadata files that contains  metadata  for each TIFF file. For example, a book bag may contain metadata files for each page image.
1. If a bag contains multiple TIFF files, these files have alphanumerically ordered names to generated the pages of books in a correct order. 
1. The islandora object metadata file (e.g., islandora book object) is also included in the bags.
