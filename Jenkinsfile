pipeline{
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts..."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Create Docker Image'){
            steps{
                sh "pwd"
                sh "ls -a"
                //sh "docker build . -t cazr32/microservices-example-1/service-001:${env.BUILD_ID}"
                sh "docker build -f Dockerfile -t cazr32/microservices-example-1/service-001:${env.BUILD_ID}" 
            }          
        }
        stage('Publish Docker Image') {
            when {
                branch 'master'
            }
            steps {
                withDockerRegistry([ credentialsId: "dockerhub-credentials", url: "" ]) {
                    sh "docker push cazr32/microservices-example-1/service-001:${env.BUILD_ID}"
                }
            }
            post {
                success {
                    sh "docker-compose up"
                }
            }
        }
    }
}