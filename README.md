# Reference Caliper Event Store

This is an implementation of a reference Caliper Event Store.

It is based on/forked from **Cube** - a system for collecting timestamped events and deriving metrics. By collecting events rather than metrics, Cube lets you compute aggregate statistics *post hoc*. It also enables richer analysis, such as quantiles and histograms of arbitrary event sets. Cube is built on [MongoDB](http://www.mongodb.org) and available under the [Apache License](/square/cube/blob/master/LICENSE).

Want to learn more about Cube? [See the wiki.](https://github.com/square/cube/wiki)

## Installation (MongoDB, Node, NPM)

### Mac OS X

#### Via [Homebrew](http://mxcl.github.com/homebrew/)
You can install MongoDB, Node.js and NPM like so: 

```bash
brew update && brew upgrade # optional but recommended
brew install node mongodb npm
```

#### Or via [Macports](http://macports.org) 
You can install MongoDB, Node.js and NPM like so: 

```bash
sudo port selfupdate
sudo port install mongodb nodejs npm
```

Cube is tested against MongoDB 2.0+ and Node 0.8+ 

### Linux

#### Ubuntu 11.04

```bash
sudo apt-get install mongodb mongodb-server
```

## Installation (Cube)

Next you’ll need to install Cube’s node modules. 

```bash
npm install cube
# OR: git clone https://github.com/square/cube.git
cd cube
npm install
```

If you're curious, the dependencies are defined in [package.json](https://github.com/square/cube/tree/master/package.json).

[[Installation on Heroku]]

## Usage

To start Cube:

```bash
mkdir -p /usr/local/var/log/cube
node bin/collector.js 2>&1 >> /usr/local/var/log/cube/collector.log &
node bin/evaluator.js 2>&1 >> /usr/local/var/log/cube/evaluator.log &
```

You can now connect to localhost:1080 for the [collector](wiki/Collector), and localhost:1081 for the [evaluator](wiki/Evaluator). You can visit <http://localhost:1081/> in your browser to see the default dashboard.

If you just want some data in order to play around with the dashboard, run the random data emitter:

```bash
node examples/random-emitter/random-emitter.js
```

You’ll want to ^C the random emitter after a few seconds. You can then reload your dashboard to see the new data.  

If your dashboard produces a 404 error, make sure you ran cube **precisely** as specified. You need to `cd` to the cube installation directory, otherwise the contents of the `static` directory are inaccessible.