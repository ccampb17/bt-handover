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

* Input pipelines
* Scraper templates
* Communicating with Anthony and reviewing

## Source completion
* Probably need a separate intro to this.



## AWS stack
Link to the BT AWS frontpage.
http://ricardo-api-dev.eu-west-1.elasticbeanstalk.com/bastiat/bt_leads_core_frontpage/

Names of instances etc
How to update using git and codepipeline
How to debug
Where the frontend is
How the classifier works
How to upload new models

A lot of the AWS stuff is more general knowledge of how Django works and is structured.