# HerokuTool

Tool for configurable one-shot deployment and db managment with heroku and rails

If you're using continuous deployment with pipelines in Heroku, you won't need this. However if that style doesn't work, then this may allow you to manage databases and deployment with more control but less hassle.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'heroku_tool'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install heroku_tool

## Configuration

> TODO: Some of these manual configuration steps, would be nicer to make a bit more automatic perhaps by making this a Rails engine.

1) Your database configuration (config/database.yml) needs to have a username if you want to use the db configurations

2) Append `load "heroku_tool/lib/tasks/db_drop_all_tables.rake"` to the end of Rakefile. 

3) Copy templates into codebase:

       cp $(gem show heroku_tool)/templates/heroku.thor ./lib/tasks
       cp $(gem show heroku_tool)/heroku_targets.yml ./config

4) update heroku_targets.yml with your staging and production targets. 
  My set up for this is to have staging deploy the local version, but production 
  deploy the origin/master.

      > TODO: more detail
 

## Usage


### Deploy 

Deploy the latest with db migrate during maintenance

    thor heroku:deploy staging

or without maintenance

    thor heroku:deploy staging --no-maintenance

or without migrating

    thor heroku:deploy staging --no-migrate
    
or a specific tag/branch

    thor heroku:deploy staging hotfix-branch


### Sync

Sync a database down from remote to local 

    thor heroku:sync:grab -f staging
    # when it finishes
    rake db:drop_all_tables && thor heroku:sync:from_dump -f staging

_FYI rake db:drop_all_tables is handy as it doesn't require you to disconnect any running processes from the database._

Copy production db to staging

    thor heroku:sync:to staging -f production

NB: this won't work from a staging to a production environment (failsafe)
    

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/red56/heroku_tool. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the HerokuTool project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/red56/heroku_tool/blob/master/CODE_OF_CONDUCT.md).
