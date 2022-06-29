# xwiki-pdf-export
Multi-page PDF export for xwiki based on application-documentation

Hi everyone, during my internship I developed a Velocity script to generate a multipage PDF export to XWiki.

This script uses the Application Documentation, please make sure you have a proper installation of this application before using the script.

## Installation

### 1 - Modify the setting.xml file in /home/.m2

Follow this link to accès the "Building XWiki from sources" page : https://dev.xwiki.org/xwiki/bin/view/Community/Building/

### 2 - Copy the maven repo locally on the server  :
```bash
rsync -avz /home/user/.m2 xwiki:/root/docker-compose/xwiki/data/.m2
```

### 3 - Ajouter les lignes suivantes dans le fichier xwiki.properties dans /xwiki/data
```
extension.repositories=maven-local:maven:file:///usr/local/xwiki/data/.m2/repository

extension.repositories=maven-xwiki:maven:http://nexus.xwiki.org/nexus/content/groups/public

extension.repositories=extensions.xwiki.org:xwiki:http://extensions.xwiki.org/xwiki/rest/
```

### 4 - Restart the docker or the XWiki server
```bash
 docker restart xwiki 
```

### 5 - Install the extension on the wiki

In "Advanced search" the extension ID and the version of the extension to install must match perfectly.

The extension ID is of this form ``groupID:artifactID``

We find these two identifiers and the version in the Maven pom.xml file of the extension

For example for the Documentation application, the extension ID is: com.xwiki.documentation:application-documentation

```
<groupId>com.xwiki.documentation</groupId>
<artifactId>application-documentation</artifactId>
<version>1.7</version>
```
#### 5.1 - SQL error after installation

It is possible that an SQL error will appear after the installation of the application on the Table Of Content page.

Expression #1 of ORDER BY clause is not in SELECT list, references column 'column_name' which is not in SELECT list; this is incompatible with DISTINCT

The problem seems to be related to the default ONLY_FULL_GROUP_BY parameter of mysql 5.7.

In this case, you need to modify the my.cnf file of your SQL server by adding :
```
[mysqld]
sql-mode=""
```
Then restart your server and the error should no longer appear.

### 6 - Create a page on xwiki outside the documentation application and paste the script in the source code of this page

## Usage

The export of attachments in the PDF is currently done in one way only.

Each attachment of the documentation pages must be imported as an attachment (e.g. [[image:archi_git_repositories_full.png]]), no attachment of another page must be used by referencing it by URL.

Be sure to have all your attachments imported into the pages 
Then simply use the xwiki PDF export

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
Please make sure to update tests as appropriate.

## License

