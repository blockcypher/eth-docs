# BlockCypher's Ethereum API Docs

This repository contains all of [BlockCypher's](http://www.blockcypher.com) Ethereum API documentation. This documentation is powered by [Slate](https://github.com/tripit/slate).

## Getting Started with BlockCypher's Ethereum API Docs

### Prerequisites

You're going to need:

 - **Linux or macOS** — Windows may work, but is unsupported.
 - **Ruby, version 2.3.1 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Clone this repository to your hard drive with `git clone https://github.com/blockcypher/eth-docs.git`
1. `cd eth-docs`
1. Initialize and start BlockCypher Ethereum Doc. You can either do this locally, or with Vagrant:

```shell
# either run this to run locally
bundle install
bundle exec middleman server

# OR run this to run with vagrant
vagrant up

# OR run this to run with docker
docker build . -t slate:latest # this only needs to be run once
docker run -p 4567:4567 -v $(pwd)/source:/srv/slate/source slate:latest
```

You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that BlockCypher Ethereum Doc is all set up on your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/slatedocs/slate/wiki/Markdown-Syntax), or [how to publish your docs](https://github.com/slatedocs/slate/wiki/Deploying-Slate).

If you'd prefer to use Docker, instructions are available [in the wiki](https://github.com/slatedocs/slate/wiki/Docker).