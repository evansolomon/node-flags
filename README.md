# Node-Flags

This is a flags library for use with [node.js](http://nodejs.org/).  Flag definitions can be distributed across multiple files, as long as they are defined before `flags.parse()` is called.

## Example

    var flags = require('flags');

    flags.defineString('name', 'Billy Noone', 'Your name');
    flags.defineInteger('age', 21, 'Your age in whole years');
    flags.defineNumber('height', 1.80, 'Your height in meters');
    flags.defineStringList('pets', []);
    flags.defineMultiString('hobby', []);

    flags.parse();

    // ====

    var info = [];
    info.push('Name : ' + flags.get('name'));
    info.push('Age : ' + flags.get('age'));
    info.push('Height : ' + flags.get('height') + '"');
    info.push('Pets : ' + flags.get('pets').join(', '));
    info.push('Hobbies : \n  ' + flags.get('hobby').join('\n  '));

    console.log(info.join('\n'));


## Defining Flags

To define flags, use one of the defineX functions exported by the `flags` module:

**flags.defineString** - Takes the raw input from the command line.

**flags.defineBoolean** - Usually doesn't take a value, passing --flag will set the corresponding flag to true.  Also supported are --noflag to set it to false. Also --flag=true or --flag=false or --flag=0 or --flag=1 or --flag=f or --flag=t 

**flags.defineInteger** - Must take a value and will be cast to a Number.  Passing a non-integer arg will throw.

**flags.defineNumber** - Must take a value and will be cast to a number.  Passing an arg that evaluates to NaN will throw.

**flags.defineStringList** - Takes a comma separated argument list and returns an array as it's value.

**flags.defineMultiString** - Same as defineString but allows multiple flags to be passed.  All values will be returned in an array.


All the define methods take the same arguments:

    flags.defineX(name, defaultValue, opt_description, opt_validator);

    name - The flag's name
    defaultValue - The default value if not specified on the command line
    description - [optional] Description to show in the help text
    validator - [optional] Function for validating the input, should throw if the input isn't valid.

## Querying Flag Values

A flag's value can be queried by either calling `flags.get('flagname')` or by querying the flags object directly `flags.FLAGS.flagname.get()`.

The flag object also contains the following properties you may be interested in:

    flag.name
    flag.defaultValue
    flag.currentValue
    flag.isSet

## Testing

By default `flags.parse` uses process.argv and slices off the first 2 elements.  For tests you can pass a predefined set of arguments as an array:

    flags.parse(['--flag', '--nofood', '--foo=bar']);

If you want to change flags between test cases, you may call:

    flags.reset();

## TODOs

 * Support --flagsfile
 * Handle --help and --helpshort internally
 * Support space separated flags, e.g. --flagname flagvalue
