# ![Logo](media/logo.png)

> Execute specified command on file change(s)

[![Travis build](https://travis-ci.org/nikersify/big-eye.svg?branch=master)](https://travis-ci.org/nikersify/big-eye)
[![AppVeyor build](https://ci.appveyor.com/api/projects/status/f6bhfklqk61bnqrc?svg=true)](https://ci.appveyor.com/project/nikersify/big-eye)
[![Coveralls](https://coveralls.io/repos/github/nikersify/big-eye/badge.svg?branch=master)](https://coveralls.io/github/nikersify/big-eye?branch=master) 

![Video](media/preview.gif)


# Install

```
$ npm install [-g] big-eye
```


# Usage

## CLI

```
$ eye --help

	Usage
	  $ eye <command>

	Options
	  -w, --watch    Files/directories to be watched [Default: pwd]
	  -i, --ignore   Files/directories to be ignored [Default: from .gitignore]
	  -l, --lazy     Don't execute command on startup
	  -d, --delay    Debounce delay in ms between command executions [Default: 100]
	  -q, --quiet    Print only command output

	Examples
	  $ eye app.js
	  $ eye build.js -w src/
	  $ eye python module.py -i '*.pyc'
	  $ eye 'g++ main.cpp && ./a.out'

	Tips
	  Run eye without arguments to execute the npm start script.
```

## API

### bigEye(file, [args], [options])

Execute `file` with `args` when a file matching the `options.watch` array
gets modified. Returns a new `Eye` instance.

#### file

Type: `String`

Absolute path to the file to be executed. Must be a non-empty `String`.

#### args

Type: `Array`

Arguments that will be passed to child process when executing `file`.

#### options

Type: `Object`

Options object that can take the following keys:

##### watch

Type: `Array`, `String`

Path(s) to files, dir(s) to be watched recursively, or glob pattern(s).

##### ignore

Type: [anymatch](https://github.com/micromatch/anymatch) compatible definition

Path(s) to files, dir(s) to be ignored, regex(es), or glob pattern(s).

##### lazy

Type: `Boolean`<br>
Default: `false`

If set to `true`, don't execute `file` after constructing the instance, but
only on watched file change.

##### delay

Type: `Number`<br>
Default: `100`

Delay in ms when debouncing execution after file changes.

### Events

Each `Eye` instance inherits from `EventEmitter` and emits a handful of events:

#### `.on('executing', ref)`

When a new child process is spawned. `ref` is an instance of
[`child_process`](http://127.0.0.1:50017/Dash/uwksyetr/nodejs/api/child_process.html#child_process_class_childprocess).

#### `on('changes', file)`

Debounced file changes that trigger spawning a child process. `file` is a path
to the file that caused the event.

#### `.on('exited', time, code)`

When a child has exited. `time` is its execution time in
miliseconds, `code` is its exit code.

#### `.on('killed', signal)`

When a child was killed due to file changes, `signal` represents the signal
used to :knife: it.


# License

MIT © [nikersify](https://nikerino.com)
