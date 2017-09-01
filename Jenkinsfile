pipeline {
	agent any

	stage('scm') {
		checkout scm
	}

	stage('clean') {
		sh "./gradlew clean --refresh-dependencies"
	}

	stage('compile') {
		sh "./gradlew compileJava"
	}

	stage('test') {
		sh "./gradlew test"
	}

	stage('check') {
		echo "todo"
	}

	stage('package') {
		sh "./gradlew build"
		// make release?
		//upload java artifact ?

		// Build docker image
		sh "./gradlew buildDocker"

		//docker push?
	}
}