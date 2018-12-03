// testuing testing
// One more test line
node{
  stage('checkout'){
	checkout([$class: 'GitSCM', branches: scm.branches, doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations, extensions: scm.extensions + [[$class: 'MessageExclusion', excludedMessage: '(?s).*test commit.*']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'c2f2562c-e361-489c-a080-366cc2a90eec', url: 'https://github.com/nagendrashiromani/devops.git']]])
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
