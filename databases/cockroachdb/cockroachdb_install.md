 
---
title : CockroachDB install
author : Mark Nielsen  
copyright : July 2024  
---


CockroachDB install
==============================

_**by Mark Nielsen
Original Copyright March 2024**_

There are two ways to install CockroachDB reasonably. First is to download CockroachDB yourself and use it for free. The second might cost a little but is almost entirely free and
if you don't go over the storage, use, and stay within the same area on GCP, it should be free forever (don't quote me on it). 


1. [Download and make Cluster on one server](#d)
    * [Install bianries](#bin)

2. [Serverless - almost free](#s)
3. [Connect to CockroachDB](#connect)

* * *
<a name=Links></a>Links
-----
* [Man pricing page](https://www.cockroachlabs.com/pricing/?utm_source=google&utm_medium=cpc&utm_campaign=g-search-na-bofu-pipe-brand&utm_term=e-cockroachdb-c&utm_content=lp660564545020&utm_network=g&_bt=660564545020&_bk=cockroachdb&_bm=e&_bn=g&gad_source=1&gclid=CjwKCAjwnK60BhA9EiwAmpHZw-orpflC7u_a-02UX6s0EP20HkXuy9E5Iiqmqe_yXe4TnFELaDdk9RoC6LMQAvD_BwE)
* [Serverless Pricing](https://www.cockroachlabs.com/pricing/?utm_source=google&utm_medium=cpc&utm_campaign=g-search-na-bofu-pipe-brand&utm_term=p-cockroach%20labs-c&utm_content=lp660564545176&utm_network=g&_bt=660564545176&_bk=cockroach%20labs&_bm=p&_bn=g&gad_source=1&gclid=CjwKCAjwnK60BhA9EiwAmpHZwwGr7pn7rtag6UR5A5Ava97MSfBOJY8ARg3U7VBAwZaMoJc7m9Q1FxoCUVcQAvD_BwE)
    * Nearest I can tell it is free forever is if you stay below 10 gis of data and 50 M RUS each month, use GCP with no cross region traffic. Then it can be good for testing.
* Convert to cockroachdb
    * [MySQL](https://www.cockroachlabs.com/docs/stable/migrate-from-mysql)
    * [PostgreSQL](https://www.cockroachlabs.com/docs/stable/migrate-from-postgres)
* CockroachDB comparisons
    * [General](https://www.cockroachlabs.com/docs/stable/cockroachdb-in-comparison)
        * This is from cockroachdb itself. Whenever companies or technologies make their own comparison chart, somehow it always benefits themselves. Not untrue, but some critical
	things might be overlooked.
    * [DBengines](https://db-engines.com/en/system/CockroachDB%3BMySQL%3BPostgreSQL)
    


* * *
<a name=d>Download and Cluster on one server</a>
-----

### Install binaries <a name=bin></a>

* https://www.cockroachlabs.com/docs/releases/
    * We will use Linux for this example.
* Download the "Full Binary" and "SQL shell Binary", this case 24.1.2
    * https://binaries.cockroachdb.com/cockroach-v24.1.2.linux-amd64.tgz
    * https://binaries.cockroachdb.com/cockroach-sql-v24.1.2.linux-amd64.tgz
```

   # make sure /usr/local/lib is in thie file
cat /etc/ld.so.conf.d/libc.conf

  # make sure /usr/local/bin and /usr/local/sbin are in the path
env | grep PATH

sudo bash

mkdir -p /usr/local/downloads
cd /usr/local/downloads

wget https://binaries.cockroachdb.com/cockroach-v24.1.2.linux-amd64.tgz
wget https://binaries.cockroachdb.com/cockroach-sql-v24.1.2.linux-amd64.tgz

tar -zxvf cockroach-sql-v24.1.2.linux-amd64.tgz
tar -zxvf cockroach-v24.1.2.linux-amd64.tgz

mv cockroach-sql-v24.1.2.linux-amd64/cockroach-sql /usr/local/bin
mv cockroach-v24.1.2.linux-amd64/cockroach  /usr/local/sbin
mv cockroach-v24.1.2.linux-amd64/lib/*  /usr/local/lib

```

* * *
<a name=s>Serverless (almost free)</a>
-----

* Signup : https://www.cockroachlabs.com/serverless/
    * Answer a bunch of questions.
    * Specify the free version.
    * Specify GCP and all in one region. GCP seems free. AWS seems to charge if you keep everything in a region
    * Add a new user
        After Adding user, click on "Connect"
        * It will ask for "Select option/language"
	    * Choose Paraemeters only and record stuff. 
	    * Choose CockroachDB Client


* * *
<a name=connect>Connect to CockroachDB serverless and your own</a>
-----
To Figure out how to connect to CockroachDB easily to the cloud or your own downloaded cluster, 
* Login into CockroachDB serverless
    * or signup https://www.cockroachlabs.com/lp/serverless/
* Click on "SQL Users
* Click on "Connect""
    * Choose the user
    * Choose how you want to connect, which OS, database, etc.
    * It will show you how to connect or what to put in your script.
    * For your own downloaded CockroachDB, use the same format. 
* To install the necessary software, use the link provided
    * https://www.cockroachlabs.com/docs/stable/connect-to-the-database.html
* Connecting to the serverless with be VERY similar to connecting to CockroachDB if you download it.
* For the client
    * FOr the cloud
        * Download https://www.cockroachlabs.com/docs/cockroachcloud/ccloud-get-started
        * Connect : ccloud cluster sql <CLUSTERNAME> -u <USERNAME> -p <ENTER-SQL-USER-PASSWORD>
    * For cloud or download
        * Install the client : https://www.cockroachlabs.com/docs/releases/
            * Choose the OS
            * It will download a zip file you will have to unzip onto your computer
                * FOr Linux/Unix/Mac: Soemething easy like /usr/local/cockrochdb
            	* For Windows:
            	    * Suggestion: Add path to environment variables.
            	    * c:\local\cockroachdb


* * *
<a name=connect>Upload file to CockroachDB</a>
-----
* Connect with CockroachDB Client