# Apache

## Look at first php files

In most cases, you will want to modify the way that Apache serves files when a directory is requested. 
After installation if a user requests a directory from the server, Apache will first look for a file called 
```index.html```. We want to tell the web server to prefer PHP files over others, so make Apache look 
for an ```index.php``` file first.

To do this, type this command to open the ```dir.conf``` file in a text editor with root privileges:

```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```
 
It will look like this:

```bash
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
 
Move the PHP index file (highlighted above) to the first position after the ```DirectoryIndex``` specification, like this:

```bash
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
 
When you are finished, save and close the file by pressing ```CTRL+X```. Confirm the save by typing ```Y```
and then hit ```ENTER``` to verify the file save location.

After this, restart the Apache web server in order for your changes to be recognized. Do this by typing this:

```bash
sudo systemctl restart apache2
```

You can also check on the status of the apache2 service using systemctl:

```bash
sudo systemctl status apache2
```
