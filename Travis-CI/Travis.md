travis CI
	>Create a .travis.yml file in the code directory.

	>sudo: required
		>Since docker requires sudo priviledge.
	>services:
	  - docker
	  	>Tells travis to check if docker is present.

	>before_install:
	  - some command
	  	>Tells travis to run this before running a test.
	  	>Since we require a docker image to test it on , we use docker build.
	  	>Command used:
	  		>docker build -t muzammilmomin/docker-react -f Dockerfile.dev .

	>script:
	  - some command
	  	>Tells travis that this is the run that we require
	  	>This script should always run a test and exits with status code success (0) ; it should not wait.
	  	>Since the existing "npm run test" waits for user input we use -- --coverage to make it run a single test and quit.
	  	>Command used :
	  		docker run muzammilmomin/docker-react npm run test -- --coverage

	 >deploy:
	 	provider: elasticbeanstalk
	 		>Use an elasticbeanstalk to deploy application.
	 	region: "ap-south-1"
	 		>Create an application on EB and use the region name generated.
	 	app: "docker-react"
	 		>The exact name of the application
	 	env: "DockerReact-env"
	 		>The environment name of the application
	 	bucket_name: "elasticbeanstalk-ap-south-1-6"
	 		>Open S3 and select the bucket associated with the project.
	 	bucket_path: "docker-react"
	 		>The name of the app
	 	on:
	 	 branch: master
	 	 	>Only deploy code if committed to master branch.
	 	aws_key_id: "Access key"
	 	secret_access_key: "Secret key"