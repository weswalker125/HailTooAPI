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
				step {
					def testFlags = "-PjvmArgs.javax.net.ssl.keystore=${env.BUILD_KEYSTORE}"
					testFlags += " -PjvmArgs.javax.net.ssl.keystorePassword=${env.BUILD_KEYSTORE_PASSWORD}"
					testFlags += " -PjvmArgs.javax.net.ssl.trustStore=${env.BUILD_KEYSTORE_PASSWORD}"
					testFlags += " -PjvmArgs.javax.net.ssl.trustStorePassword=${env.BUILD_KEYSTORE_PASSWORD}"
					sh "./gradlew test"
				}
			}
		}

		stage('check') {
			steps {
				step {
					echo "todo"
				}
			}
		}

		stage('package') {
			steps {
				step {
					sh "./gradlew build"
				}
				// make release?
				//upload java artifact ?

				step {
					// Build docker image
					sh "./gradlew buildDocker"
				}

				//docker push?
			}
		}

		stage('deploy') {
			when {
				branch 'production'
			}
			steps {
				step {
					echo 'todo - deploy to production machine'
				}
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