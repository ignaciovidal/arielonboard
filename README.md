## The Hub


### Systems Acquisition, Development and Maintenance Policy

Before you begin to work with this code base, please review the [Systems Acquisition, Development and Maintenance Policy](https://github.com/Hall-and-Partners/hall-and-partners/blob/master/ISMS-C_DOC_14.1.pdf) so you understand the requirements around systems changes for this project.


### Code Standards

[Coding standards](https://github.com/Hall-and-Partners/hall-and-partners/blob/master/code-standards.md) are documented in this file. This should give the general idea and approach that we prefer to follow. If you're approaching something new and need direction or an example of where something similar may have been implented, just ask another developer for ideas. In general our preference is that we ask if we're not 100% certain. Feedback from another developer is always helpful.


### Set up development environment

Install Python dependencies:

    $ git clone https://github.com/Hall-and-Partners/hall-and-partners.git
    $ cd hall-and-partners
    $ pipenv install --dev

If you have problems installing pycrypto, on the mac you can include the `/usr/local/include` path:

    $ export "CFLAGS=-I/usr/local/include -L/usr/local/lib"

Install Redis: If you're on a Mac, you can do this using homebrew with the following Command.

    $ brew install redis

Install FFmpeg:

    $ brew install ffmpeg --with-openssl

Install ElasticSearch 7.x (with the ingest-attachment plugin): If you're on a Mac, you can do this using homebrew with the following commands.

    $ brew tap elastic/tap
    $ brew install elastic/tap/elasticsearch-full
    $ brew services start elastic/tap/elasticsearch-full
    $ brew install elastic/tap/kibana-full
    $ elasticsearch-plugin install ingest-attachment
    $ brew services restart elastic/tap/elasticsearch-full

To prepare your schema in elasticsearch run:

    $ python manage.py search_index_rebuild -d

If you actually want to ingest something you’ll need to run:

    $ python manage.py simple_search_ingest

There’s a lot of parameters you can pass to limit the scope of what gets ingested. To see them run:

    $ python manage.py simple_search_ingest --help

To generate export images you will need to have phantomjs installed. The easiest way to do that is to use homebrew if you're on a Mac:

    $ brew tap homebrew/cask
    $ brew cask install phantomjs

To support single sign on you will need to install libxml. You can do this on a Mac with homebrew:

    $ brew install libxmlsec1

If you have problems getting `python-saml` installed or it's dependency `dm.xmlsec.binding` you may need to reset your Xcode command line tools:

    $ sudo xcode-select -s /Library/Developer/CommandLineTools

Setup Git Hooks: Connecting this will automattically check `git commit` with the `flake8` command
and will error if there are any issues.

    $ git config core.hooksPath ./git_hooks/


### To seed the database

We use PostgreSQL 10 for the backend database. If you don't have PostgreSQL installed do the following:

    $ brew install postgresql@10

You will need to follow the homebrew configuration commands after the installation.

You'll need to request a database url link in order to download a recent snapshot of the datbase. (Note that psql.gz is replaced with the actual filename that was provided to you.)

    $ curl -O psql.gz
    $ gunzip psql.gz
    $ dropdb hall
    $ createdb hall
    $ psql -d hall < psql
    $ python manage.py migrate


### Start the lightweight development server included with Django.

    $ ./manage.py runserver

### Start the Redis Sever.

    $ redis-server

### Run tests (ha)

    $ flake8
    $ py.test

### Deploying

Setup deployment with `eb init`.

* Default region = eu-west-1
* Python version = 3.7
* No Codecommit
* No SSH

Set your default enviroment. E.g.

    eb use hall-develop

Deploy the branch you're on.

    eb deploy

There's a deploy script in the `bin` directory. You use it like so:

    ./bin/deploy.sh --develop
    ./bin/deploy.sh --staging
    ./bin/deploy.sh --production


### Branches

* `master` is always deployed to production
  (https://data.thehubinsights.com).
* `staging` is always deployed to staging
  (https://staging-data.thehubinsights.com).
* `develop` is always deployed to develop
  (https://develop-data.thehubinsights.com).

### To deploy to an environment not tied to a branch

    eb deploy <environment-name>
    eb deploy hall-worker-priority
    eb deploy hall-worker


### Current environment names

    hall-develop
    hall-develop-worker
    hall-staging-3
    hall-staging-worker
    hall-production-1
    hall-worker-1
    hall-worker-priority-1

