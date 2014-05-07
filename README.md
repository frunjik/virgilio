# Virgilio
[![Stuff to do](https://badge.waffle.io/icemobilelab/virgilio.png)](https://waffle.io/icemobilelab/virgilio)

Virgilio is a minimalist library for helping you write modular applications.

## Getting started
The following creates a Virglio instance and tells it to load the number module.

    var options = {};
    require('virgilio')(options)
        .loadModule(number);

Below is an example of what a number module might look like.

    function number(options) {
        var virgilio = this;
        virgilio.namespace('number')
            .defineAction('add', add)
            .defineAction('subtract', subtract);

        function add(num1, num2) {
            return (num1 + num2);
        }

        function subtract(num1, num2) {
            var virgilio = this;
            return virgilio.execute('number.add', num1, -num2);
        }
    }

Then somewhere else, you might call:

    virgilio.execute('number.subtract', 8, 3)
        .then(function(result) {
            console.log(result) //5
        }).done();

## API
### Virgilio( [options] )
Create a virgilio instance.
The optional options object is shared among all registered modules.

### modulesvirgilio.loadModule( module )
Load a module.
A virgilio module is a function that takes a single options argument.
The function is bound to the virgilio instance and instantly called.

### virgilio.defineAction( actionName, actionHandler )
Register an action with virgilio.
The `actionName` is automatically scoped to the namespace it was called from.

### virgilio.execute( actionName [, arg1, arg2, ...])
Execute an action with certain arguments.
`actionName` is automatically scoped to the namespace it was called from.

`execute` always returns a promise for the result of the action, even if the action itself directly returns a value!

### virgilio.namespace( name )
Get a reference to a certain namespace.
If it doesn't exist, it is created for you.
The returned namespace-object proxies all virgilio methods.
This method allows you to write:

    virgilio.defineAction('foo.bar.sub.run')
            .defineAction('foo.bar.sub.stop');

as:

    virgilio.namespace('foo.bar.sub')
        .defineAction('run')
        .defineAction('stop');

### virgilio.log
A bunyan instance to use for logging.
For instance:

    virgilio.log.info('Everything running smoothly!');
    virgilio.log.error('Ouch');

## Options
The options object passed to Virgilio is shared with all modules and can thus be used for configuring the entire app.
Virgilio itself reads only the logger property:

    var options = {
        logger: { level: 10, name: 'virgilio' } //defaults
    }

To see what options the logger supports, check out the bunyan documentation.

## Grunt
The Virgilio project comes with a couple of grunt tasks:

- `grunt test` runs all tests.
- `grunt docs` generates the annotated sourcecode webpage in the `docs/` directory.



We dedicate this Library to the ServiceRegistrar, the EigenServices and the PuppetDresser.
