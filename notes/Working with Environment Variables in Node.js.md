---
title: Working with Environment Variables in Node.js
created: '2019-09-04T10:17:56.255Z'
modified: '2019-09-04T10:18:06.465Z'
---

# Working with Environment Variables in Node.js

![h6p92574BnyPzrK_d-MEH0rJ7nVcFBfbPfOXlnf5tWFT12Y74mxqvutrSBRw3ntDM4es5ThipSUtWr3SafnUd27s1-gcRU1JURKJxbNfPrvbQDCDr8Uri4OP4rNNf5fcWJSs6w3k](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/h6p92574BnyPzrK_d-MEH0rJ7nVcFBfbPfOXlnf5tWFT12.width-808.png)

[Working with environment variables](https://www.twilio.com/blog/2017/01/how-to-set-environment-variables.html) is a great way to configure different aspects of your [Node.js](https://nodejs.org/) application. Many cloud hosts (Heroku, Azure, AWS, now.sh, etc.) and Node.js modules use environment variables. Hosts, for example, will set a `PORT` variable that specifies on which port the server should listen to properly work. Modules might have different behaviors (like logging) depending on the value of `NODE_ENV` variable.

Here are some of my tricks and tools when working with environment variables in Node.js.

### The Basics

Accessing environment variables in Node.js is supported right out of the box. When your Node.js process boots up it will automatically provide access to all existing environment variables by creating an `env` object as property of the `process` global object. If you want to take a peek at the object run the the Node.js REPL with `node` in your command\-line and type:

console.log(process.env);

This code should output all environment variables that this Node.js process is aware of. To access one specific variable, access it like any property of an object:

console.log('The value of PORT is:', process.env.PORT);

You should see that the value of `PORT` is `undefined` on your computer. Cloud hosts like Heroku or Azure, however, use the `PORT` variable to tell you on which port your server should listen for the routing to work properly. Therefore, the next time you set up a web server, you should determine the port to listen on by checking `PORT` first and giving it a default value otherwise:

const app \= require('http').createServer((req, res) \=> res.send('Ahoy!')); const PORT \= process.env.PORT || 3000;   app.listen(PORT, () \=> {
 console.log(\`Server is listening on port ${PORT}\`); });

The highlighted line will take the value of the `PORT` if it’s available or default to `3000` as a fallback port to listen on. Try running the code by saving the it in a file like `server.js` and run:

node server.js

The output should be a message saying `Server is listening on port 3000`. Stop the server with `Ctrl+C` and restart it with the following command:

PORT\=9999 node server.js

The message should now say `Server is listening on port 9999` since the `PORT` variable has been temporarily set for this execution by the `PORT=9999` in front of `node`.

Since `process.env` is simply a normal object we can set/override the values very easily:

process.env.MY\_VARIABLE \= 'ahoy';

The code above will set or override the value of `MY_VARIABLE`. However, keep in mind that this value is only set during the execution of this Node.js process and is only available in child processes spawned by this process. Overall you should avoid overriding environment variables as much as possible though and rather initialize a config variable as shown in the `PORT` example.

### Explicitly loading variables from `.env` files

If you develop on multiple different Node.js projects on one computer, you might find you have overlapping environment variable names. For example, different messaging apps might need different Twilio Messaging Service SIDs, but both would be called `TWILIO_MESSAGE_SERVICE_SID`. A great way to achieve project specific configuration is by using `.env` files. These files allow you to specify a variety of different environment variables and their values.

Typically you don’t want to check these files into source control so when you create one you should add `.env` to your your `.gitignore`. You will see in a lot of Twilio demo applications `.env.example` files that you can then copy to `.env` and set the values yourself. Having an `.env.example` or similar file is a common practice if you want to share a template file with other people in the project.

How do we load the values from this file? The easiest way is by using an [npm module called `dotenv`](http://npm.im/dotenv). Simply install the module via npm:

npm install dotenv \-\-save

Afterwards add the following line to the very top of your entry file:

require('dotenv').load();

This code will automatically load the `.env` file in the root of your project and initialize the values. It will skip any variables that already have been set. You should not use `.env` files in your production environment though and rather set the values directly on the respective host. Therefore, you might want to wrap your load statement in an if\-statement:

if (process.env.NODE\_ENV !== 'production') {
 require('dotenv').load(); }

With this code we will only load the `.env` file if the server isn’t started in production mode.

Let’s see this in action. Install `dotenv` in a directory as shown above. Create an `dotenv-example.js` file in the same directory and place the following lines into it:

console.log('No value for FOO yet:', process.env.FOO);   if (process.env.NODE\_ENV !== 'production') {
 require('dotenv').load(); }   console.log('Now the value for FOO is:', process.env.FOO);

Afterwards, create a file called `.env` in the same directory with this content:

FOO\=bar

Run the script:

node dotenv\-example.js

The output should look like:

No value for FOO yet: undefined Now the value for FOO is: bar

As you can see the value was loaded and defined using `dotenv`. If you re\-run the same command with `NODE_ENV` set to `production` you will see that it will stay `undefined`.

NODE\_ENV\=production node dotenv\-example.js

Now the output should be:

No value for FOO yet: undefined Now the value for FOO is: undefined

If you don’t want to modify your actual code you can also use Node’s `-r` argument to load `dotenv` when executing the script. Change your `dotenv-example.js` file:

console.log('The value for FOO is:', process.env.FOO);

Now execute the file first normally:

node dotenv\-example.js

The script should print that the current value for `FOO` is `undefined`. Now execute it with the appropriate flag to require `dotenv`:

node \-r dotenv/config dotenv\-example.js

The result is that `FOO` is now set to `bar` since the `.env` file has been loaded.

[If you want to learn more about `dotenv` make sure to check out its documentation.](https://www.npmjs.com/package/dotenv)

### An alternative way to load `.env` files

Now `dotenv` is great but there was one particular thing that bothered me personally during my development process. It does not override existing env variables and you can’t force it to do so.

As a result of these frustrations, I decided to write my own module based on `dotenv` to fix this problem and make loading environment variables more convenient things. The result is [`node-env-run` or `nodenv`](http://npm.im/node-env-run). It’s a command\-line tool that will load a `.env` file, initialize the values using `dotenv` and then execute your script.

You can install it globally but I recommend to only use it for development purposes and locally to the project. Install it by running:

npm install node\-env\-run \-\-save\-dev

Afterwards create a file `nodenv-example.js` and place this code in it:

console.log('The value for FOO is:', process.env.FOO);

As you can see, we don’t need to require anything here. It’s just the application logic. First try running it using `node`:

node nodenv\-example.js

This executed code should output `The value for FOO is: undefined`. Now try using `node-env-run` by running:

node\_modules/.bin/nodenv nodenv\-example.js

The result should be `The value for FOO is: bar` since it loaded the `.env` file.

`node-env-run` can override existing values. To see it in action first run the following command:

FOO\=foo node\_modules/.bin/nodenv nodenv\-example.js

The command\-line output should say `The value for FOO is: foo`. If we now enable force mode we can override existing values:

FOO\=foo node\_modules/.bin/nodenv \-\-force nodenv.js

As a result we should be back to the original `The value for FOO is: bar`.

If you want to use this tool regularly, I recommend that you wrap it into an npm script. By adding it in the `package.json` like so:

{
 "name": "twilio\-blog", "version": "1.0.0", "description": "", "main": "nodenv\-example.js", "scripts": { "start": "node .", "start:dev": "nodenv \-f ." }, "author": "", "license": "ISC", "devDependencies": { "node\-env\-run": "^2.0.1" } }

This way you can simply run:

npm run start:dev

[If you want to learn more about `node-env-run` make sure to check out its documentation.](https://www.npmjs.com/package/node-env-run)

### Environment variables && npm scripts

There are scenarios where it’s useful to check the value of an environment variable before entering the Node.js application in npm scripts. For example if you want to use `node-env-run` when you are in a development environment but `node` when you are in `production` mode. [A tool that makes this very easy is `if-env`](http://npm.im/if-env). Install it by running:

npm install if\-env \-\-save

Make sure to not install it as a “dev dependency” since we will require this in production as well.

Now simply modify your npm scripts in your `package.json`:

 "scripts": { "start": "if\-env NODE\_ENV=production ?? npm run start:prod || npm run start:dev", "start:dev": "nodenv \-f .", "start:prod": "node ." }

This script will now execute `npm run start:prod` and subsequently `node .` if `NODE_ENV` has the value `production` and otherwise it will execute `npm run start:dev` and subsequently `nodenv -f .`. You can do this technique with any other environment variable as well.

Try it out by running:

# should output "The value of FOO is: bar" npm start # should output "The value of FOO is: undefined" NODE\_ENV\=production npm start

[If you want to learn more about `if-env` make sure to check out its documentation.](https://npm.im/if-env)

### Debugging

We all know the moment where things are just not working the way you want it to and some module is not doing what it’s supposed to do. That’s the moment it’s time for debugging. One strategy that helped me a lot is using the `DEBUG` environment variable to receive more verbose logs for a bunch of modules. If you for example take a basic `express` server like this:

const app \= require('express')(); const bodyParser \= require('body\-parser'); app.use(bodyParser.json()); // for parsing application/json app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x\-www\-form\-urlencoded   app.post('/', (req, res, next) \=> { console.log(req); });   app.listen(3000);

And start it up with the `DEBUG` variable set to `*`:

DEBUG\=\* node server.js

You’ll receive a bunch of extensive logging that looks something like this:

![Screen Shot 2017-08-10 at 3.14.45 PM.png](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/hIGBRIJamWZBmUDg8ceCSzEfFCb0Cv3A5TE1fuS_WoZlbq.width-500.png)

The “magic” behind it is a [lightweight module called `debug`](http://npm.im/debug) and its usage is super easy. When you want to use it all you have to do is to initialize it with a “namespace”. Afterwards you can log to that namespace. If someone wants to see the output all they have to do is enable the namespace in the `DEBUG` variable. `express` in this case uses a bunch of sub\-namespaces. If you would want everything from the `express` router, all you have to do is set `DEBUG` with the appropriate wildcard:

DEBUG\=express:router\* node server.js

If you want to use `debug` in your own module all you have to do is to first install it:

npm install debug \-\-save

And afterwards consume it the following way:

const debug \= require('debug')('myApp:someComponent');   debug('Here is a pretty object %o', { someObject: true });

[If you want to learn more about `debug` make sure to check out its documentation.](https://www.npmjs.com/package/debug)

### Now use all the environment variables!

These are by far not all the things you can do with environment variables and all the tools that exist but rather just the ones that I use most often. If you have any other great tools that you regularly use, I would love to hear about them!

*   Email: dkundel@twilio.com
*   Twitter: [@dkundel](https://twitter.com/dkundel)
*   GitHub: [dkundel](https://github.com/dkundel)

*Now that you’ve learned a bit more about Environment variables in Node.js, try out Twilio’s [Node.js quickstarts](https://www.twilio.com/docs/quickstart/node)!*

[Working with Environment Variables in Node.js - Twilio](https://www.twilio.com/blog/2017/08/working-with-environment-variables-in-node-js.html)


