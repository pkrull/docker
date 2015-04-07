# Dockerfile

This is a dockerfile that builds and runs an application that is built using the
"angular-fullstack" yeoman generator: https://github.com/DaftMonk/generator-angular-fullstack
and then had typescript added to the project.  

So this dockerfile builds a project that has the following stack:
* node
* grunt
* typescript
* bower

Files assumed to be in root:
* `package.json`
* `bower.json`
* `tsd.json`
* `Gruntfile.js`


For this Dockerfile to work, you have to make one modification to the generated
`Gruntfile.js`.

You must remove the `open` task from the `serve:dist` registered task.
