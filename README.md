# docker-w3c-html5-validator
Docker image for W3C HTML 5 validator for use by grunt-html-angular-validate task

## Building
To build the Docker image, run the following:

```sh
$ ./build.sh
```

This will take some time (lots to download), performing the following tasks:
- Install Ubuntu base (15.10), Apache HTTP server, OpenJDK 8, [supervisord](http://supervisord.org/) and a few others.
- Download W3C validator and [Validator.nu](http://validator.github.io/validator/) `vnu.jar` portable HTML5 validator jar (version 15.6.29).
- Install and configure W3C validator (including Validator.nu setup).
- Start Apache and Validator.nu under `supervisord`.

That's the boring/messy stuff out of the way - your new Docker image should now be built.

Alternatively you can pull this image directly from the Docker Hub Registry:

```sh
$ docker pull setelis/w3c-html5-validator
```

## Running
To start the image, run the following:

```sh
$ docker run -d setelis/w3c-html5-validator
```

This will start the image in a new container, you can use these ports:
- Port `80` (Apache2) to host machine on port `8080`.
- Port `8888` (Validator.nu Java server) to host on port `8888`.

To view IP address of the container:
```sh
$ docker ps
$ docker inspect <<<container name get from docker ps>>>
```

Then you can deploy/attach somewhere in your infrastructure...

## Grunt task
You can use validator as follows:
 - package.json (0.3.0 version is mentioned, it is tested with 0.5.0, but 0.5.8 does not work - maybe this is a bug in html-angular-validator):
 ```
 "grunt-html-angular-validate": "~0.3.0"
 ```
 - Gruntfile.js:
 ```
 htmlangular: {
	 options: {
		 angular: false,
		 charset: "utf-8",
		 wrapping: {
			 "tr": "<table>{0}</table>"
		 },
		 w3clocal: "ip-address"
	 },
	 continuous: {
		 options: {
			 tmplext: ".html"
		 },
		 files: {
			 src: ["<%= files.templates %>"]
		 }
	 },
 ```

## Credits
Image is builded on top of:
 - https://github.com/magnetikonline/dockerhtml5validator
 - http://blog.simplytestable.com/installing-the-w3c-html-validator-with-html5-support-on-ubuntu/
