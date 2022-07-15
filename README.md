# Yrb

Yrb is a Ruby binding for Y-CRDT. It provides distributed data types that enable
real-time collaboration between devices. Yrb can sync data with any other
platform that has a Y-CRDT binding, allowing for seamless cross-domain
communication.

The library is a thin wrapper around Yrs, taking advantage of the safety and
performance of Rust.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'y-rb'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install y-rb

> **⚠ WARNING: Partial support for pre-built binaries.**  
> `y-rb` comes with experimental pre-built binaries for a few platforms, but it
> is not possible to support all targets right now. Please make sure there is an
> archive that matches the OS, CPU architecture, and Ruby version. You can find
> the assets listed with each [release](https://github.com/y-crdt/yrb/releases).
>
> If the platform is not listed, you need to add the latest stable Rust + Cargo
> to your build dependencies in order for `y-rb` to install properly.

## Usage

```ruby
# creates a new document and text structure
local = Y::Doc.new 
local_text = local.get_text("my text")

# add some data to the text structure
local_text.push("hello")  
  
# create a remote document sharing the same text structure
remote = Y::Doc.new 
remote_text = remote.get_text("my text")  

# retrieve the current state of the remote document
remote_state = remote.state  

# create an update for the remote document based on the current
# state of the remote document
update = local.diff(remote_state)  
  
# apply update to remote document
remote.sync(update)  

puts remote_text.to_s == local_text.to_s # true  
```  

More [examples](docs/examples.md).

## Development

Make sure you have `cargo` available (2021 edition). The gem needs the lib to
be built every time when there is a change.

```bash
cargo build --release
```

After checking out the repo, run `bin/setup` to install dependencies. Then,
run `rake spec` to run the tests. You can also run `bin/console` for an
interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`.
To release a new version, update the version number in `version.rb`, and then
run `bundle exec rake release`, which will create a git tag for the version,
push git commits and the created tag, and push the `.gem` file to
[rubygems.org](https://rubygems.org).

## Docs

You can run `yard` locally and open the docs with:

```bash
yard server 
open "http://0.0.0.0:8808/"
```

## Decision log

For this `gem`, we maintain a [decision log](docs/decisions.md). Please consult it
in case there is some ambiguity in terms of why certain implementation details
look as they are. 

## License

The gem is available as open source under the terms of the
[MIT License](https://opensource.org/licenses/MIT).
