pipeline {
	agent any

	stages {
		stage('scm') {
			steps {
				checkout scm
			}
		}

		stage('clean') {
			steps {
				sh "./gradlew clean --refresh-dependencies"
			}
		}

		stage('compile') {
			steps {
				sh "./gradlew compileJava"
			}
		}

		stage('test') {
			environment {
				BUILD_KEYSTORE = credentials('build_keystore')
				BUILD_KEYSTORE_PASSWORD = credentials('build_keystore_password')
			}
			steps {
				sh "./gradlew test -PjvmArgs.javax.net.ssl.keystore=${env.BUILD_KEYSTORE} -PjvmArgs.javax.net.ssl.keystorePassword=${env.BUILD_KEYSTORE_PASSWORD} -PjvmArgs.javax.net.ssl.trustStore=${env.BUILD_KEYSTORE_PASSWORD} -PjvmArgs.javax.net.ssl.trustStorePassword=${env.BUILD_KEYSTORE_PASSWORD}"
			}
		}

		stage('check') {
			steps {
				echo "todo"
			}
		}

		stage('package') {
			steps {
				// make release?
				//upload java artifact ?

				// Build docker image
				sh "./gradlew containerize"
				//docker push?
			}
		}

		stage('deploy') {
			when {
				branch 'production'
			}
			steps {
				echo 'todo - deploy to production machine'
			}
		}
	}
	post {
		always {
			echo "done"
		}
		changed {
			echo 'todo - notify slack channel'

			emailext (
				subject: '$DEFAULT_SUBJECT',
				body: '$DEFAULT_CONTENT',
				recipientProviders: [[$class: 'DevelopersRecipientProvider']]
			)
		}
	}
}