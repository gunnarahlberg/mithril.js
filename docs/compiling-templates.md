## Compiling Templates

If performance is absolutely critical, you can optionally pre-compile templates that use `m()` by running the [`template-compiler.sjs`](tools/template-compiler.sjs) macro with [Sweet.js](https://github.com/mozilla/sweet.js)

Compiling a template transforms the nested function calls into a raw virtual DOM tree (which is merely a collection of native Javascript objects that is ready to be rendered via [`m.render`](mithril.render.md))

For example, given the following template:

```javascript
var view = function() {
	return m("a", {href: "http://google.com"}, "test");
}
```

It would be compiled into:

```javascript
var view = function() {
	return {tag: "a", attrs: {href: "http://google.com"}, children: "test"};
}
```

Note that compiled templates are meant to be generated by an automated build process and are not meant to be human editable.

---

### Installing NodeJS and SweetJS for one-off compilations

SweetJS requires a [NodeJS](http://nodejs.org) environment. To install it, go to its website and use the installer provided.

To install SweetJS, NodeJS provides a command-line package manager tool. In a command line, type:

```
npm install -g sweet.js
```

To compile a file, type:

```
sjs --module /mithril.compile.sjs --output <output-filename>.js <input-filename>.js
```

---

### Automating Compilation

If you want to automate compilation, you can use [GruntJS](http://gruntjs.com), a task automation tool. If you're not familiar with GruntJS, you can find a tutorial on their website.

Assuming NodeJS is already installed, run the following command to install GruntJS:

```
npm install -g grunt-cli
```

Once installed, create two files in the root of your project, `package.json` and `Gruntfile.js`

`package.json`

```javascript
{
	"name": "project-name-goes-here",
	"version": "0.0.0", //must follow this format
	"devDependencies": {
		"grunt-sweet.js": "*"
	}
}
```

`Gruntfile.js`

```javascript
module.exports = function(grunt) {
	grunt.initConfig({
		sweetjs: {
			modules: ["mithril.compile.sjs"],
			compile: {expand: true, cwd: ".", src: "**/*.js", dest: "destination-folder-goes-here/"}
		}
	});
	
	grunt.loadNpmTasks('grunt-sweet.js');

	grunt.registerTask('default', ['sweetjs']);
}
```

Make sure to replace the `project-name-goes-here` and `destination-folder-goes-here` placeholders with appropriate values.

To run the automation task, run the following command from the root folder of your project:

```
grunt
```

More documentation on the grunt-sweet.js task and its options [can be found here](https://github.com/natefaubion/grunt-sweet.js)
