// testuing testing
// One more test line
private boolean lastCommitIsVersionCommit() {
		lastCommit = sh([script: 'git log -1', returnStdout: true])
		if (lastCommit.contains("Verion changed")) {
			return true
		} else {
			return false
		}
	}
node{
  stage('checkout'){
	//checkout([$class: 'GitSCM', branches: scm.branches, doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations, extensions: scm.extensions + [[$class: 'MessageExclusion', excludedMessage: '.*shiromani.*']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'c2f2562c-e361-489c-a080-366cc2a90eec', url: 'https://github.com/nagendrashiromani/devops.git']]])
	checkout scm
	if (lastCommitIsVersionCommit()) {
        currentBuild.result = 'ABORTED'
        error('Last commit bumped the version, aborting the build to prevent a loop.')
    } else {
        echo('Last commit is not a bump commit, job continues as normal.')
    }

  } 
 stage('set-env'){
    env.JAVA_HOME="${tool 'JDK-1.8'}"
    env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
    sh 'java -version'
}

  stage('Build') {
     mvnHome = tool 'M3' 
     // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   } 
}
