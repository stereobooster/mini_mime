# MicroMime

This is the fork of [MiniMime](https://github.com/discourse/mini_mime) with C extension ported from [mime-types-mini](https://github.com/ioquatix/mime-types-mini/).

Minimal mime type implementation for use with the mail and rest-client gem.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'micro_mime'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install micro_mime

## Usage

```
require 'micro_mime'

MicroMime.lookup_by_extension("txt").content_type
# => "text/plain"

MicroMime.lookup_by_content_type("text/plain").extension
# => "txt"

MicroMime.lookup_by_content_type("text/plain").binary?
# => false

```

## Performance

MicroMime is optimised to minimize memory usage. It uses C extension and ideal hash function generated by gperf. There are benchmarks in the [bench directory](https://github.com/stereobooster/micro_mime/bench/bench.rb)

```
Memory stats for requiring mime/types/columnar
Total allocated: 10706215 bytes (114133 objects)
Total retained:  3472281 bytes (31906 objects)

Memory stats for requiring micro_mime
Total allocated: 33442 bytes (182 objects)
Total retained:  4653 bytes (14 objects)
Warming up --------------------------------------
content_type lookup MicroMime
                       111.490k i/100ms
content_type lookup Mime::Types
                        17.279k i/100ms
Calculating -------------------------------------
content_type lookup MicroMime
                          1.777M (± 8.6%) i/s -      8.808M in   5.003187s
content_type lookup Mime::Types
                        237.226k (± 7.9%) i/s -      1.192M in   5.061223s
```

As a general guideline, cached lookups are 2x faster than MIME::Types equivelent. Uncached lookups are 10x slower.


## Development

MicroMime uses the officially maintained list of mime types at [mime-types-data](https://github.com/mime-types/mime-types-data)repo to build the internal database.

To update the database run:

```ruby
bundle exec rake rebuild_db
```

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/discourse/mini_mime. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

