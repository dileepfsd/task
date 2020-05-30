jenkinsPipeLine {

     stage('Step 1 : Git Checkout') {
       git credentialsId: 'Dtp@1611', url: 'https://github.com/dileepfsd/task.git'
       echo 'Done : Git checkout'
     }

   stage('Step 2 : Build Image') {
         sh 'docker build -t thattan79/task-":$BUILD_NUMBER"'
    }

   stage('Step 3 : Deliver Image') {
	   	withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerPwd')]) {
	    	sh "docker login -u thattan79 -p ${DockerPwd}"
		}
   		sh 'docker push thattan79/task-":$BUILD_NUMBER"'
   }

   stage('Step 4 : Run Image') {

   //MySql Standalone
        sh 'docker run -p 3306:3306 --name mysql-standalone -e MYSQL_ROOT_PASSWORD=admin -e MYSQL_DATABASE=cogdb -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -d mysql:latest'

       // Spring boot application
         sh 'docker run -p 8080:8080 --name task-":$BUILD_NUMBER"--link mysql-standalone:mysql -d task-":$BUILD_NUMBER"'
      }
}
