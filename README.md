# Pando Handbook

Pando is a decentralized computing commandline tool that enables a stream of values to be processed collaboratively by volunteers on the web.

This handbook shows how to install and use pando, as well as how to reproduce experiments performed for submitted and published papers. If you obtained this handbook from a publication archive, it is actually a valid Git repository. It can therefore be updated to the latest version on GitHub (http://github.com/elavoie/pando-handbook) by doing `git pull origin master`.

# Installation

Using NPM:

    npm install -g pando-computing
    
Latest Development Version:

    git clone https://github.com/elavoie/pando-computing
    cd pando-computing
    npm install && npm link

# Quick Start
   
Save the following code in a file named `square.js` (or alternatively use the one provided in the pando-computing repository under `examples/square.js`) to define a computing function:

    module.exports['/pando/1.0.0'] = function (x, cb) {
      setTimeout(function () {
        x = Number.parseInt(JSON.parse(x))
        var r = x * x
        cb(null, r)
      }, 1000)
    }

You can square a list of numbers:

    pando square.js 1 2 3 4 5 6 7 8 9 10

On startup, Pando should print on the standard error (your ip address will be different):

    Serving volunteer code at http://192.168.16.194:5000
    Serving monitoring page at http://192.168.16.194:5001
    
Then, Pando should print on the standard output the following numbers, one per second:

    1
    4
    9
    16
    
You can accelerate the computation by opening multiple browser tabs using the url for the volunteer code (`http://192.168.16.194:5000` in the previous example).

You can ask pando to read the inputs from the standard input rather than as arguments:

    seq 1 10 | pando square.js --stdin

And you can combine with other post-processing to keep only results that are even:

    seq 1 10 | pando square.js --stdin | grep -e '.*[02468]$'
    

You can also use other npm packages, such as the `debug` package, in your computing function:

    var debug = require('debug')
    var log = debug('pando:examples:square')

    module.exports['/pando/1.0.0'] = function (x, cb) {
      log('started processing ' + x)
      setTimeout(function () {
        x = Number.parseInt(JSON.parse(x))
        var r = x * x
        log('returning ' + r)
        cb(null, r)
      }, 1000)
    }

You then simply have to install the packages used in the current working directory (ex: `npm install debug`). When loading the computing function on startup, Pando automatically packages it with all its dependencies with browserify so that it runs in the browser.

# Supported Browsers

All Browsers that support WebRTC should be able to connect and execute volunteer code. So far we have succesfully used the latest versions of:

* Firefox
* Chrome


# Publication-Specific Instructions for Reproducing Experiments

These instructions are aimed at Artifact Evaluation Committee (AEC) members and other researchers and developers that wish to verify our claims as well as expand upon our work. Each publication is documented in its own directory, with all the data we obtained during our own experiments, the software and hardware configuration that were used, and the helper scripts we used. The specific version of Pando and the version of this handbook used are mentioned for each publication for replicability while allowing non-backward compatible changes to Pando in later versions.

* [OOPSLA 2017](./oopsla-2017)



# Related Packages




