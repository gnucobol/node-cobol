<!---------------------------------------------------------------------------->
<!-- STOP, LOOK & LISTEN!                                                   -->
<!-- ====================                                                   -->
<!-- Do NOT edit this file directly since it's generated from a template    -->
<!-- file, using https://github.com/IonicaBizau/node-blah                   -->
<!--                                                                        -->
<!-- If you found a typo in documentation, fix it in the source files       -->
<!-- (`lib/*.js`) and make a pull request.                                  -->
<!--                                                                        -->
<!-- If you have any other ideas, open an issue.                            -->
<!--                                                                        -->
<!-- Please consider reading the contribution steps (CONTRIBUTING.md).      -->
<!-- * * * Thanks! * * *                                                    -->
<!---------------------------------------------------------------------------->

![cobol](http://i.imgur.com/DutRzDr.png)

# cobol [![Donate now][donate-now]][paypal-donations]

COBOL bridge for NodeJS which allows you to run COBOL code from NodeJS.

## Can I use this on production?
Of course, you can! It's production ready! If you ever did such a thing, [ping me (@IonicaBizau)](https://twitter.com/IonicaBizau). :boom: :dizzy:

## Installation

Currently GNUCobol is required. If you are using a debian-based distribution you can install it using:

```sh
$ sudo apt-get install open-cobol
```

It would be interesting to fallback into a COBOL compiler written in NodeJS. [Contributions are welcome!][contributing] :smile:

Then, install the `cobol` package.

```sh
$ npm i cobol
```

## Example

```js
// Dependencies
var Cobol = require("cobol");

// Execute some COBOL snippets
Cobol(function () { /*
       IDENTIFICATION DIVISION.
       PROGRAM-ID. HELLO.
       ENVIRONMENT DIVISION.
       DATA DIVISION.
       PROCEDURE DIVISION.

       PROGRAM-BEGIN.
           DISPLAY "Hello world".

       PROGRAM-DONE.
           STOP RUN.
*/ }, function (err, data) {
    console.log(err || data);
});
// => "Hello World"

Cobol(__dirname + "/args.cbl", {
    args: ["Alice"]
}, function (err, data) {
    console.log(err || data);
});
// => "Your name is: Alice"

// This will read data from stdin
Cobol(function () { /*
       IDENTIFICATION DIVISION.
       PROGRAM-ID. APP.
      *> http://stackoverflow.com/q/938760/1420197

       ENVIRONMENT DIVISION.
       INPUT-OUTPUT SECTION.
       FILE-CONTROL.
       SELECT SYSIN ASSIGN TO KEYBOARD ORGANIZATION LINE SEQUENTIAL.

       DATA DIVISION.
       FILE SECTION.
       FD SYSIN.
       01 ln PIC X(64).
          88 EOF VALUE HIGH-VALUES.
       WORKING-STORAGE SECTION.
       PROCEDURE DIVISION.
       DISPLAY "Write something and then press the <Enter> key"
       OPEN INPUT SYSIN
       READ SYSIN
       AT END SET EOF TO TRUE
       END-READ
       PERFORM UNTIL EOF
       DISPLAY "You wrote: ", ln
       DISPLAY "------------"
       READ SYSIN
       AT END SET EOF TO TRUE
       END-READ
       END-PERFORM
       CLOSE SYSIN
       STOP RUN.
*/ }, {
    stdin: process.stdin
  , stdout: process.stdout
}, function (err) {
    if (err) {
        console.log(err);
    }
});
// => Write something and then press the <Enter> key
// <= Hi there!
// => You wrote: Hi there!
// => ------------

```

## Documentation

### `Cobol(input, options, callback)`
Runs COBOL code from Node.JS side.

#### Params
- **Function|String|Path** `input`: A function containing a comment with inline COBOL code, the cobol code itself or a path to a COBOL file.
- **Object** `options`: An object containing the following fields:
- **Function** `callback`: The callback function called with `err`, `stdout` and `stderr`.

## How to contribute
Have an idea? Found a bug? See [how to contribute][contributing].

## License
[KINDLY][license] © [Ionică Bizău][website]–The [LICENSE](/LICENSE) file contains
a copy of the license.

[Heading image source](http://www.nyhistory.org/sites/default/files/press/hr/1.5.2c%20GraceHopperCobolCHM_102722559.03.01-HR%2BP.src_.jpg)–Thanks! :cake:

[license]: http://ionicabizau.github.io/kindly-license/?author=Ionic%C4%83%20Biz%C4%83u%20%3Cbizauionica@gmail.com%3E&year=2015
[contributing]: /CONTRIBUTING.md
[website]: http://ionicabizau.net
[docs]: /DOCUMENTATION.md
[paypal-donations]: https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=MG98D7NPFZ3MG
[donate-now]: http://i.imgur.com/jioicaN.png