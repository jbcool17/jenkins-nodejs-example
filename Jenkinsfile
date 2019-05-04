podTemplate(label: 'node-build-pod', containers: [
  containerTemplate(name: 'git', image: 'alpine/git', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'node', image: 'node:8.16.0-alpine', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
]
) {
  node('build-pod') {
    stage('Clone repository') {
            container('git') {
                sh 'whoami'
                sh 'hostname -i'
                sh 'git clone -b master https://github.com/jbcool17/jenkins-nodejs-example.git'
            }
      }
      stage('Node Build') {
          container('node') {
            dir('jenkins-nodejs-example/') {
              sh 'node -v'
              sh 'npm -v'
              sh 'ls -lah'
            }
          }
      }
      stage('Check running containers') {
          container('docker') {
            dir('jenkins-nodejs-example/') {
              sh 'docker build -t node-test:v1 .'
              sh 'docker tag nodejs-test:v1 jbcool17/nodejs-test:v1'
          
            }
          }
      }
  }
}
