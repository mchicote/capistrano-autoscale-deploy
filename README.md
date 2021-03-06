[![Gem Version](https://badge.fury.io/rb/capistrano-autoscale-deploy.png)](http://badge.fury.io/rb/capistrano-autoscale-deploy)
# Capistrano AutoScale Deploy

Capistrano plugin that enable discovery of instances in ec2 AutoScale groups into capistrano deployment script.

## Requirements

* aws-sdk
* capistrano ~> 2.15.5


## Installation

Add this line to your application's Gemfile:

    gem 'capistrano-autoscale-deploy'

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install capistrano-autoscale-deploy

Add this line to your application's Capfile:

```ruby
require 'capistrano/autoscale_deploy'
```

And set credentials with AmazonEC2ReadOnlyAccess permission in config/deploy.rb as:

```ruby
set :aws_default_region, 'eu-west-1'
set :aws_access_key_id, "YOUR AWS KEY ID"
set :aws_secret_access_key, "YOUR AWS SECRET KEY"

```

## Usage

In the capistrano deploy script / stage files add the following line

```ruby
autoscale :AutoScaleGroup => 'name of AutoScale group (by Name tag)', :deploy_roles => [:app, :web, :db, :primary => true]
```

you can add more autoscale configs to deploy to multiple autoscale groups like a cluster

## How this works

This gem will fetch only running instances that have an autoscale tag name you specified

It will then reject the roles of :db and the :primary => true for all servers found **but the first one** 

(from all autoscale groups you have specified such as using more then once the autoscale directive in your config - i.e cluster deploy)

this is to make sure a single working task does not run in parallel

you end up as if you defined the servers yourself like so:

````ruby
server ip_address1, :app :db, :web, :primary => true
server ip_address2, :app, :web
server ip_address3, :app, :web
````

## Contributing

1. Fork it ( http://github.com/<my-github-username>/capistrano-autoscale-deploy/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
