Use-case
========

You want to run sql/plpgsql code on multiple versions of PostgreSQL to see
how it performs on all of them.
          
This software allows you to create a build farm using docker containers
with a different PostgreSQL version on each container. The builds will
be made using the PostgreSQL source from upstream.

This can be used as a testing environment to test PostgreSQL
features/queries/sql/plpgsql over multiple different major and minor
PostgreSQL versions.

First, it creates docker containers pre-provisioned with the PostgreSQL source code.
Then, inside each of them, the appropriate Pg version is built, and then a Pg server is
started inside each of them.

Then you can run a test suite over multiple Pg versions and then build reports based
on Pg logs to see how your test case behaved on different versions.

On these containers **/var/lib/postgresql/** is the datadir for Pg, and the logs are stored in
**/var/log/postgresql/logfile** .

Usage
=====
    
    ./op make_image # build common docker image used for all containers
    ./op make_containers # create all the containers from the image we just made
    ./op make_pg # builds PostgreSQL with different versions on all the containers
    ./op make_pg_cluster # basically runs initdb on each container to create a data
                         # directory, and also provisions them with a postgresql.conf
                         # config file
    ./op start_pg # starts the PostgreSQL server on all containers
    ./op run_tests # runs the tests we have lined up in the ./t/ directory on all
                   # the containers

    ./op report # get a report for each of the tests

Misc
====

    ./op shell_pg 9.6.8 # gives you a shell to the docker container that's running version 9.6.8
                        # under the postgres user
    ./op shell_root 9.6.8 # same as before but with the root user

The naming convention for containers is `pg-<x.y.z>` if they contain version `x.y.z` of PostgreSQL.

Other similar projects
======================

This project can be run on a single machine. I believe the official PostgreSQL
[build farm can be found here](https://github.com/PGBuildFarm). But I imagine it must be
more complicated to set up (probably can do more things as well).



