def pipelineContext = [:]
node {

    def registryProjet='registry.gitlab.com/takman/20-jenkins'
    def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

    echo "IMAGE = $IMAGE"

    stage('Clone') {
        checkout scm
    }

    def img = stage('Build') {
        docker.build("$IMAGE",  '.')
    }
	
    stage('Run') {
        img.withRun("--name run-$BUILD_ID -p 50080:80") { c ->
            sh 'curl localhost:50080'
        }					
    }

    stage('Push') {
        docker.withRegistry('https://registry.gitlab.com', 'd3665a11-ff83-40ed-8962-c414b4673991') {
            img.push 'latest'
            img.push()
        }
    }
}

