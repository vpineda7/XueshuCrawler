# XueshuCrawler
Based on `python3.6` and `scrapy` framework,crawl information from `Baidu Scholar`(百度学术),include Database(`Mysql` and `Mongodb`),`Json` pipelines and dynamic content fetching(use `PhantomJS`)<br>
You should first install python3.6 and PhantomJS compatible to your system first.

[PhantomJS](http://phantomjs.org/download.html)

## Install requirements
```Bash
pip install -r requirements.txt
```

For windows users,when installing `Scrapy`,you may need to compile some frameworks(like `twisted`) from source.So you should have Visual Studio installed first.<br>Or you can download a compiled version(.whl file) from
[Unofficial Windows Binaries for Python Extension Packages](http://www.lfd.uci.edu/~gohlke/pythonlibs/)<br>

(`Note:`As a windows user,you also should download a `pywin32` whl file from the link above and install it,<br>then execute `python your-python-install-path\Scripts\pywin32_postinstall.py -install`)

## Configuration

First,login into you mysql account and execute the following commands to create a new table:<br>

        CREATE TABLE xueshu (
        id int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
        title varchar(60) UNIQUE NOT NULL,
        author varchar(60) NOT NULL,
        publish varchar(40) DEFAULT NULL,
        year year(4) DEFAULT NULL,
        cite int(11) DEFAULT NULL,
        subject varchar(120) DEFAULT NULL,
        abstract text
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8

Then,Open the `xueshu\settings.py` file and 
Edit the following variables to fit your environments:<br>


* `MYSQL_HOST`: your mysql database's hostname
* `MYSQL_USER`: your mysql database's username
* `MYSQL_PASS`: your mysql database's password
* `MYSQL_DB`: the database that stores your data
* `MONGO_URI`:the mongodb's uri(like `'host:port'`)
* `MONGO_DB`：the mongodb's database name
* `MONGO_COLLECTION`:the mongodb's collection name
* `ALL_PAGE`: the number of pages you want to fetch
* `PHANTOM_PATH`:the path of PhantomJS

<br>To search another keyword,you can change line 19 of `xueshu\xueshu_spider.py`,
from '非织造' to another keyword.

If you want to enable Json file output and mysql database,please remove comments of the related lines.The default pipeline is Mongodb.

## Run

```Bash
scrapy crawl xueshu`
```

This can fetch lots of information about papers from Baidu Scholar,include title,author,publish year,cited number,subjects,abstracts and so on.
The data was stored to a Json file and also your Mysql database.
It uses PhantomJS headless browser to render JavaScript and get the abstracts of papers.

```Bash
cd visualization &&
python plot.py
```
Use `wordcloud` to render the key words of papers fetched before

<img width="320" height="480" src="https://raw.githubusercontent.com/rollingstarky/XueshuCrawler/master/visualization/wordcloud.png"/>







