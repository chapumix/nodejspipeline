node {
   def gitcommit
   stage('Verificación SCM') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     gitcommit = readFile('.git/commit-id')
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
   stage('Docker Build & Push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("wardviaene/docker-nodejs-demo:${gitcommit}", '.').push()
     }
   }
}
