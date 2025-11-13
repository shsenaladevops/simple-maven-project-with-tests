node {
  checkout scm
  sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
  junit '**/target/surefire-reports/TEST-*.xml'
}
