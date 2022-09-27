# Building the docs

## Requirements
* Python 3.10
* Doxygen (can be the portable version)

## Steps

### 1 - Cloning the repo
---
Clone the repo and navigate to the `docs` folder.
```bat 
git clone https://github.com/ShoosGun/SlateShipyard.git

cd SlateShipyard/docs
```

### 2 - Installing the python libraries
---
This docs are generated with Menagerie, so to be able to use generate the docs you need to do
```py
pip install menagerie-docs
```
If while you are trying to build the docs it fails in `MDPage.py`, try to go to an older version of markdown libs with
```py
py -m pip uninstall markdown && py -m pip install markdown==3.3.7
```
### 3 - Generating new docs from comments in code
---
#### 3.1 - Generating new xml files
To generate xml files from code comments open `doxywizard.exe` from your Doxygen install and with it, on the folder `docsxml`, open the `Doxyfile` file. Then go to the `Run` tab and press `Run doxygen`, if there are new comments to be turned into documentation it will override or add the files to the folder `docsxml/xml`.

#### 3.2 - Generating new md files
To generate the md files open have the `docsxml/md` and `docsxml/xml` already open. Inside the `docsxml/md` there is a .md file called `apiTemplate.md`, that is the file which will be used as the template for the docs, it uses a combination of [Scriban](https://github.com/scriban/scriban) and [Markdig](https://github.com/xoofx/markdig)(some markdig calls can be called to turn md to hmtl if needed using Scriban syntax) to read and generate the documentation from the xml generated by doxygen. 

Now run the executable in `docsxml/XmlToMenagerieParser/bin/XmlToMenagerieParser.exe`, it will first ask for the **absolute** path of the `docsxml/xml` folder (so copy it and paste it on the program) and then it will ask for the **absolute** path of the `docsxml/md` folder, after pressing the last enter it will run will either anwsering that it is done, or with an exception, which could be caused by an exception originated from the scriban script on the template, so if you modified it and it failed try to find where it is faling with the exception information.

#### 3.3 - Copying the newly generated files

The files that `XmlToMenagerieParser.exe` generates are found on the `docsxml/md`, so copy them (but not the template folder) and move them to `docs/content/pages/documentation`.

### 4 - Generating the static website files
---
Before we can call menagerie to generate the files you need to first, using the terminal, go to the `docs` folder, and then set the enviroment variable `URL_PREFIX` to `"/"`, to do this run on the terminal
```sh
$Env:URL_PREFIX="/"
```
Now you can run `generate` from menagerie in python

```py
menagerie generate
```
### 5 - Testing locally the static website
---
To locally test the website go to the now generated `docs/out` using the terminal and there run
```py
http.server 8080
```
You can then view the website by entering `http://localhost:8080/` on a browser.

## Before pushing changes in a pull request
---
Before you decide to do a pull request on changes you made to the website, remember to delete the `docs/out` folder, as it will be automatically generated by github