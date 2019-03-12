********************
Answers Engine Fetch
********************

Fetch is a platform where you can scrape data from the internet quickly and easily without incurring significant costs.
We do this by allowing you to scrape data from the shared-cache of contents that Fetch has collectively downloaded from the Internet for other users to scrape.
Your scraping from the cached-content does not necessarily prevent you from getting the freshest content. In fact, you can specify how fresh the contents you want to scrape are by specifying your freshness-type and also specifying that the content should be “force fetched” or not.

Keep in mind that any page that Fetch downloads for your scraper, will be stored on the shared-cache, and will be available to other Fetch users to use for their own scrapers. This reuse of cached pages allows every Fetch users to collectively save cost and save time as they scrape data from the Internet.
Fetch is like curl, where you have lower level control of HTTP request, such as request method, headers, body, and more.

Getting Started
===============

In this getting started section, we will get you started with installing the necessary requirements, and then deploying and running an existing scraper into AnswerEngine Fetch. Currently we support ruby 2.4.4 and 2.5.3.

Install AnswersEngine Command Line Interface using rubygems
-----------------------------------------------------------

.. code-block:: bash

   $ gem install answersengine
   Successfully installed answersengine-0.2.3
   Parsing documentation for answersengine-0.2.3
   Done installing documentation for answersengine after 0 seconds
   1 gem installed

Get your access token
---------------------

You can create “account_admin” or “basic” token.
The difference between the two is an “account_admin” token can create other access tokens, whereas basic account could not.

Set environment variable of your access token
---------------------------------------------

.. code-block:: bash

   $ export ANSWERSENGINE_TOKEN=<your_token_Here>

Now you’re ready to go.

Create the scraper
------------------

In this step we will create a scraper on AnswersEngine, by specifying the scraper name, and the git repository where the scraper script comes from

.. code-block:: bash

   $ answersengine scraper create summitprice1 git@git.answersengine.com:scrapers/summitracing-prices.git --workers 1
   {
    "name": "summitprice1",
    "id": 18,
    "account_id": 1,
    "force_fetch": false,
    "freshness_type": "any",
    "created_at": "2018-11-26T03:28:22.037768Z",
    "git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
    "git_branch": "master",
    "deployed_git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
    "deployed_git_branch": "master",
    "deployed_commit_hash": "e7d77d7622e7b71c32300eafd2d44a8429142fe3",
    "deployed_at": "2018-11-26T04:58:29.922676Z",
    "worker_count": 1,
    "config": {
     "parsers": [
      {
       "file": "./parsers/part.rb",
       "page_type": "part"
      }
     ],
     "seeder": {
      "file": "./seeder/seeder.rb"
     }
    }
   }

Let’s see if your scraper has been created.
Let’s look at the list of scrapers that you have now:

.. code-block:: bash

   $ answersengine scraper list
   [
    {
     "name": "ebay",
     "id": 20,
     "account_id": 1,
     "force_fetch": false,
     "freshness_type": "any",
     "created_at": "2018-11-26T22:00:43.007755Z",
     "git_repository": "https://github.com/answersengine/ebay-scraper.git",
     "git_branch": "master",
     "deployed_git_repository": "https://github.com/answersengine/ebay-scraper.git",
     "deployed_git_branch": "master",
     "deployed_commit_hash": "7bd6091d97a17cf8ee769e00ac285123c41aaf4f",
     "deployed_at": "2018-11-28T06:13:56.571052Z",
     "worker_count": 1,
   ...

Or if you’d like to see your specific scraper, you can do:

.. code-block:: bash

   $ answersengine scraper show summitprice1
   {
    "name": "summitprice1",
    "id": 18,
    "account_id": 1,
    "force_fetch": false,
    "freshness_type": "any",
    "created_at": "2018-11-26T03:28:22.037768Z",
    "git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
    "git_branch": "master",
    "deployed_git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
   ...

Now that we have created the scraper, we need to deploy.

Deploying the scraper
---------------------

Once we have created the scraper, let’s deploy it from the git repo that you have specified.

.. code-block:: bash

   $ answersengine scraper deploy summitprice1
   Deploying scraper. This may take a while...
   {
    "id": 135,
    "scraper_id": 18,
    "commit_hash": "e7d77d7622e7b71c32300eafd2d44a8429142fe3",
    "git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
    "git_branch": "master",
    "errors": null,
    "success": true,
    "created_at": "2018-11-28T15:10:53.374802Z",
    "config": {
     "parsers": [
      {
       "file": "./parsers/part.rb",
       "page_type": "part"
      }
     ],
     "seeder": {
      "file": "./seeder/seeder.rb"
     }
    }
   }

Let’s see if the list of deployments, if you’re curious to know your deployment history.

.. code-block:: bash

   $ answersengine scraper deployment list summitprice1
   [
    {
     "id": 135,
     "scraper_id": 18,
     "commit_hash": "e7d77d7622e7b71c32300eafd2d44a8429142fe3",
     "git_repository": "git@git.answersengine.com:scrapers/summitracing-prices.git",
     "git_branch": "master",
   ...

Run the scraper
---------------

Now that the scraper codes has been deployed, let’s run it.

.. code-block:: bash

   $ answersengine scraper start summitprice1
   Starting a scrape job...
   {
    "id": 93,
    "scraper_id": 18,
    "created_at": "2018-11-28T15:11:56.657034Z",
    "freshness": null,
    "force_fetch": false,
    "status": "active",
    "seeding_at": null,
    "seeding_failed_at": null,
    "seeded_at": null,
    "seeding_try_count": 0,
    "seeding_fail_count": 0,
    "seeding_error_count": 0,
    "worker_count": 1
   }

This will now then create a scraping job, which will start fetching pages for you, and parsing them into the outputs.

You can also see all jobs that was created on the scraper.

.. code-block:: bash

   $ answersengine scraper job list summitprice1
   [
    {
     "id": 93,
     "scraper_name": "summitprice1",
     "scraper_id": 18,
     "created_at": "2018-11-28T15:11:56.657034Z",
   ...

To view the current job on the scraper.

.. code-block:: bash

   $ answersengine scraper job show summitprice1
   {
    "id": 93,
    "scraper_name": "summitprice1",
    "scraper_id": 18,
    "created_at": "2018-11-28T15:11:56.657034Z",
   ...

Viewing the Job Stats
---------------------

While the job is running, let’s look how the job is doing by looking at the stats. You’ll first need to get the ID form the job list command above.

.. code-block:: bash

   $ answersengine scraper stats summitprice1
   {
    "job_id": 93,
    "pages": 822,
    "fetched_pages": 822,
    "to_fetch": 0,
    "fetching_failed": 0,
    "fetched_from_web": 0,
    "fetched_from_cache": 822,
    "parsed_pages": 0,
    "to_parse": 822,
    "parsing_failed": 0,
    "outputs": 0,
    "output_collections": 0,
    "workers": 1,
    "time_stamp": "2018-11-28T15:13:17.437174Z"
   }

Viewing the Job Pages
---------------------

Let’s see the pages that has been added by the seeder script into this job.

.. code-block:: bash

   $ answersengine scraper page list summitprice1
   [
    {
     "gid": "www.summitracing.com-ffac364b31eb3484a6a952c1c273144a",
     "job_id": 93,
     "page_type": "part",
     "method": "GET",
     "url": "http://www.summitracing.com/parts/avs-25544",
     "effective_url": "https://www.summitracing.com/parts/avs-25544",
   ...

Viewing a Global Page Content
-----------------------------

You may be wondering what is a Global Page.
A Global Page acts like a shared-cache that AnswersEngine fetches for all their users as they perform scraping. This shared-cache allows every users to collectively benefit from lower cost and higher performance of extracting data from the Internet.

Now that you’ve seen the pages that has been added into this job, let’s see the content of the page by copying and pasting a page’s GID(Global ID) into the following command.

.. code-block:: bash

   $ answersengine globalpage content www.summitracing.com-9038c8a06696a3053cbcb393e78ceef4
   Preview content url: "https://fetch.answersengine.com/public/global_pages/preview/HS2RNNi0uKe2YQ3tlU-cedGCWhRHgLcm5PWTwTVx0VLs5yjlOt6bE8qma7lzv6oCfUSYBNHu3IpXK70961lRhcqruPg5xa29OmuSJvolz_ONcVV2nmeMfJx8tSe_jRi8JW1qIfD7O8Rchf3XdO10pfjgICiV_FBczWPGYmg3rNLGcHMk5UGseJcl7maAGvN5bhvrwesscrODp_mni894gKz8a9v3GTFtjVGUgexS-dEu2DKTfe6SNb1ZKHj08SUCTM61P_Umg6XzF-bJBePMZuoX2b8nkXQ3mDw1-bdMJ-WPFUfQ01T5gtkoCBDuSFBg-T8YGETNEPNm0usglfWzsq4="

View the scraper output
-----------------------

Job Outputs are stored in collections. If none is specified, it will be stored in the “default” collection.
Let’s view the outputs of a scraper job by first seeing what collections the scraper outputs to:

.. code-block:: bash

   $ answersengine scraper output collection summitprice1
   [
    {
     "collection": "parts",
     "count": 14
    }
   ]

In the result of the command line above, you will see the collection called “parts”. Let’s look at the outputs inside the “parts” collection :

.. code-block:: bash

   $ answersengine scraper output list summitprice1 --collection parts
   [
    {
     "AAIABrandCode": "BBCG",
     "ManufacturerPartNumber": "10530",
     "PartNumber": "10530",
     "Price": "$15.99",
     "ProductLine": "Air Lift Air Line Cutters",
     "SitePartNumber": "AIR-10530",
     "TimeStamp": "2018-11-12T23:16:20.248094Z",
     "Title": "Air Lift Air Line Cutters 10530",
     "UPC": "729199105302",
     "URL": "http://www.summitracing.com/parts/air-10530",
     "_collection": "parts",
     "_created_at": "2018-11-28T15:13:38.400001Z",
     "_gid": "www.summitracing.com-d0279bd12cf9f2dc5968b6a56aa20104",
     "_id": "df638cb643414074ac32dd8fc9a106f2",
     "_job_id": 93
    },
   ...

View the scraper logs
---------------------

If there is an error that occured it will be shown in the job log.
Let’s see what’s in the log.

.. code-block:: bash

   $ answersengine scraper log summitprice1

You can view the log of what happens.

Congratulations! You’ve created and ran your first scraper.

Let’s now cleanup from this Getting Started section by canceling that running job.

.. code-block:: bash

   $ answersengine scraper job cancel summitprice1
   {
    "id": 93,
    "scraper_name": "summitprice1",
    "scraper_id": 18,
    "created_at": "2018-11-28T15:11:56.657034Z",
    "freshness": null,
    "force_fetch": false,
    "status": "cancelled",
    "seeding_at": "2018-11-28T15:12:14.388469Z",
    "seeding_failed_at": null,
    "seeded_at": "2018-11-28T15:13:23.9002Z",
    "seeding_try_count": 1,
    "seeding_fail_count": 0,
    "seeding_error_count": 0,
    "worker_count": 1
   }

You’re now done with the Getting Started section. Next steps are to read the high level concepts, and do the tutorials.
