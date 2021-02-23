
1

I had a similar symptom. In my case though, my idiocy was in unintentionally also having an empty index.html file in the web root folder. Apache was serving this rather than index.php when I didn't explicitly request index.php, since DirectoryIndex was configured as follows in mods-available/dir.conf:
