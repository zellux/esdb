### Introduction

ESDB is the API server for GGTracker.  It is also involved in replay processing.

The other codebases used in GGTracker are:
* https://github.com/dsjoerg/ggtracker <-- the web server and HTML/CSS/Javascript
* https://github.com/dsjoerg/ggpyjobs <-- the replay-parsing python server
* https://github.com/dsjoerg/gg <-- little gem for accessing ESDB

It's not ready for public consumption yet.  Don't read this.  Please delete your computer.


### Requirements

 * Ruby 1.9+ (get RVM: https://rvm.io/)
 * Bundler 1.2.0+ (gem install bundler --pre)
 * MySQL
 * Memcached
 * Redis
 * Curl
 * imagemagick (http://www.imagemagick.org/)
 * An Amazon S3 account
 
On Mac OSX, you can use homebrew as package manager: http://mxcl.github.com/homebrew/


### Installation and setup

 * Run Bundler (`bundle`)
 * Copy and adjust database configuration (`cp config/database.yml.example config/database.yml`)
 * Create the database esdb needs, and the test database (`mysql -u root` and then `create database esdb_development; create database esdb_test` and then `quit`)
 * Copy S3 configuration and put your AWS credentials and bucket names in it (`cp config/s3.yml.example config/s3.yml`)
 * Copy fog configuration and put your AWS credentials and bucket names in it (`cp config/fog.rb.example config/fog.rb`)
 * Copy and adjust redis configuration (`cp config/redis.yml.example config/redis.yml`)
 * Copy and adjust esdb configuration (`cp config/esdb.yml.example config/esdb.yml`)
 * Copy tokens configuration (`cp config/tokens.yml.example config/tokens.yml`)
 * Run migrations: `bundle exec sequel -m db/migrations -e development config/database.yml`
 * Import the data needed for Spending Skill: `cat db/replays_sq_skill_stat.sql | mysql -u root -D esdb_development`
 * Set up an API identity for the ggtracker web server: `cat db/ggtracker_provider.sql | mysql -u root -D esdb_development`
 * Initialize the ggpyjobs submodule with `rake py:init`
 * Run migrations on test: `bundle exec sequel -m db/migrations -e test config/database.yml`
 * Verify your install with rspec `bundle exec rspec`


### Starting

`foreman start`


### Testing

bundle exec rspec


### Security

If you see any security problems, please let me know ASAP!  Thx :)
