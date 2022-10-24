node() {
    checkout scm
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    try {
        stage('Test') {
            docker.image('qnib/pytest').inside {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } 
        }
    } finally {
        junit 'test-reports/results.xml'
    }
    stage('Deploy') {
        withEnv(['VOLUME=$(pwd)/sources:/src',
        'IMAGE=cdrx/pyinstaller-linux:python2']) {
            sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
            archiveArtifacts 'sources/dist/add2vals'
            sh "printf 'yes\n' | scp -i /var/jenkins_home/workspace/submission-cicd-pipeline-naufalhanif1477/EC2-DevOps-Key-Pair.pem /var/jenkins_home/workspace/submission-cicd-pipeline-naufalhanif1477/sources/dist/add2vals  ec2-user@18.143.118.131:/home/ec2-user/build"
	        sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
        }
    } 
}
