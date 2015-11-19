# Reference Caliper Event Store

This is an implementation of a reference Caliper Event Store.  It is used to collect Caliper Events and store them into a backing database ([MongoDB](http://www.mongodb.org)).

It is based on/forked from [Cube](https://github.com/square/cube) - a system for collecting timestamped events and deriving metrics. 

By collecting events rather than metrics, Cube lets you compute aggregate statistics *post hoc*. It also enables richer analysis, such as quantiles and histograms of arbitrary event sets. Cube is built on [MongoDB](http://www.mongodb.org) and available under the [Apache License](/square/cube/blob/master/LICENSE).

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

## Usage

To start Cube:

```bash
mkdir -p /usr/local/var/log/cube
node bin/collector.js 2>&1 >> /usr/local/var/log/cube/collector.log &
node bin/evaluator.js 2>&1 >> /usr/local/var/log/cube/evaluator.log &
```

You can now connect to localhost:1080 for the [collector](wiki/Collector), and localhost:1081 for the [evaluator](wiki/Evaluator). You can visit <http://localhost:1081/> in your browser to see the default dashboard.

To send sample Caliper Events, you can

```
cd examples/caliper-events
```

then

```
curl -X POST -d @caliper-event.json http://localhost:1080/1.0/event/put
```

This will add the event into a MongoDB collection (identified by the value of the "sensor" attribute in the event envelope).

The default dashboard will show you a simple horizon chart so you can see metrics on the number of events going in.

If you would like to introspect and/or work with the events, then you can go against the MongoDB collection and retrieve events.  There are plenty of MongoDB libraries available, include for e.g. [Mongoose](http://mongoosejs.com/) if you like working with node.js.
 
Alternatively, you can use Cube's [evaluator](https://github.com/square/cube/wiki/Evaluator) with the appropriate (event or metric) query to get statistics on your event data.

If your dashboard produces a 404 error, make sure you ran cube **precisely** as specified. You need to `cd` to the cube installation directory, otherwise the contents of the `static` directory are inaccessible.