# Hello

## Switch/Dropbox based stack
This is the part that has to be replaced at some point by AWS, so read these with a view on replacing them.

Parts to be aware of:
### cronjobs
`/cron`
This is where the cronjobs are executed. Most of them are basic wrappers that execute scripts held elsewhere. I found this to be the most reliable way to manage and do things.
ASIS structure documentation
https://crontab.guru/

### Scraper execution
GTA:
/code/daily/running

DPA
/code/BT-Lumiere scrapers/scrapers

### Individual scraper structure

### bt_sync_221_main()

### other gtabastiat functions
Most of these are now deprecated. Some of them are still in use, particularly the above and some of the snippety-but-useful functions like `bt_guess_country()` and `bt_guess_date()`. For those, the package documentation explains their use very well.

### The stock data
Explain how the stock data is stored and loaded (will be replaced by direct DB access, in some cases this is already partially implemented)?

### Emailers
Explain emailer scripts and how these (are supposed to) work.

### Scraper maintenace/common issues
Now that you have a grasp on the general stack structure, here I list some common problems and issues that can cause the routine to break down.

## Input pipelines and working with Anthony
Link to Anthony's github repo:
https://github.com/global-trade-alert/anthony_osei

Main pipeline for Anthony:
https://docs.google.com/spreadsheets/d/1suAzHpHkiGB142AZonr2_1_6zEY_zjPeLdhJ-3s7HgE/edit#gid=864363600

Input pipeline filled out by DPA editors:
https://docs.google.com/spreadsheets/d/12Zm93SpquTiYbyARCMzKq3x9Eh4ZkvZ-h8S2EtmXHas/edit#gid=1321738403

Process summary:
1. Editors add links to the pipeline.
2. You send them to Anthony by copying them into his pipeline sheet. Usually I would do max 10 at a time to prevent overloading him (and making sure he always has something to do).
3. He will notify you when he's done them and if he has any questions or issues. You should go through each one and check it's getting the correct results - often differences occur just due to links being accessed from a different location.
4. When reviewed, you can give feedback to Anthony. Then remove the files from the 'in review' folder and put them in the 'finished' folder. Anthony knows what he is doing, so 99% of the time it's fine. But on occasion he may miss something (as we would all do when making so many scrapers). Always good to have two sets of eyes.
5. Also put the files in the relevant dropbox folders (GTA: `code/daily/running`; DPA: `code/BT-Lumiere scrapers/scrapers`). When they are synced to the server, they should automatically start running.


## AWS stack
Link to the BT AWS frontpage.
http://ricardo-api-dev.eu-west-1.elasticbeanstalk.com/bastiat/bt_leads_core_frontpage/

### Things to bear in mind:
1. Names of instances:
	* EB name: ricardo-api-development
	* EB link: http://ricardo-api-dev.eu-west-1.elasticbeanstalk.com/

2. How to update using git and codepipeline
	When you push to the relevant branch, the deployment process begins. You can check this on codepipeline.
	Remember that each time you do this, you must SSH into the instance (`ssh -i "[...]\GTA cloud\setup\keys\ricardo.pem" ec2-user@ec2-34-244-73-255.eu-west-1.compute.amazonaws.com`) and run this script to recopy the required files that are not part of the repo:
	`/usr/share/bastiat/add_bt_files.sh`
3. How to debug
	The stdout from the instance can be found by running:
	`less /var/log/web.stdout.log`
	Then pressing ctrl+end to get to the bottom and see what it is (or isn't) doing.
4. Where the frontend is
	https://github.com/global-trade-alert/ricardo-api/tree/development/src/apps/bastiat/templates
5. How the classifier works
	I think you know this better than me ;)
6. How to upload new models
	You can putty (or whatever) into the instance and put the files directly in the relevant dir: `/usr/share/bastiat/best_model/...`



## Source collection

The source collection is a combination of four pieces:
1. `bt_sa_record_new_source()`: Updates the relevant tables in the DB. Takes new URLs from state acts and adds them to the relevant source collector tables.
2. `bt_store_sa_source()`: The workhorse function of the process. Calls the above to update the tables, then calls the below to scrape a source, before saving the outcome of the scrape in the tables, and/or updating the URLs for redirects etc  (for a full breakdown, please refer to the code).
3. `bt_collect_url()`: Checks the URL's status, then tries to either scrape it or download it (if it's a file).
4. `rasterize.js`: A java-based applet by Patrick that uses phantomjs to PDF-ise a webpage at a high resolution. Because phantomjs is deprecated, this part of the process can have several flavours of failure and could probably benefit from improvement the most.

Directly relevant DB tables:
`gta_files`
`gta_url_log`
`gta_measure_url`
`gta_url_status_list`