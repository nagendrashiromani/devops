import org.jenkinsci.plugins.workflow.job.WorkflowJob

Jenkins.instance.getAllItems(WorkflowJob.class).each {
  	println(it.fullName)
  it._getRuns().each{ buildLog ->
      println(buildLog.getDisplayName()+" :: "+buildLog.getLogText().length())
  }
};

// testuing testing
// One more test line
node{
  stage('checkout'){
	checkout scm
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
