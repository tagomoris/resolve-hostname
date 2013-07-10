# Resolve::DNS::Cached

`Resolve::DNS::Cached` is hostname resolver with:

* caching with specified TTL
* backend swithing between built-in `IPScoket.getaddress` and extended module `resolv`
* support of IPv6 address resolving and rereading of resolver setting (ex: /etc/resolv.conf)
  * for only `resolv` backend

## Installation

Add this line to your application's Gemfile:

    gem 'resolve-dns-cached'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install resolve-dns-cached

## Usage

    require 'resolve/dns/cached'
    
    resolver = Resolve::DNS::Cached.new
    resolver.resolve('www.google.com') #=> "173.194.72.103"
    resolver.resolve('www.google.com') #=> "173.194.72.103" # from cache

Cache TTL(seconds) setting available for each resolver instances like this:

    r = Resolve::DNS::Cached.new(:ttl => 10) # default 60 seconds

    r.resolve('www.example.com')
    sleep 11
    r.resolve('www.example.com') # not from cache, but from actual dns record (and cached)

Ruby DNS resolver implementation `resolv` module is available for backend of this module. You can specify `resolver_ttl` with expectation for re-loading of '/etc/resolve.conf' in long life daemons.

    r = Resolve::DNS::Cached.new(:ttl => 10, :resolv => true, :resolver_ttl => 20)
    r.resolve('www.example.com')

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## TODO

* negative cache support
  * configuration to return nil instead of to raise errors
* DNS round robin support
  * only for `resolv` backend

## Copyright

* [MIT License](http://www.opensource.org/licenses/MIT).
* Copyright (c) 2013- TAGOMORI Satoshi (tagomoris)
