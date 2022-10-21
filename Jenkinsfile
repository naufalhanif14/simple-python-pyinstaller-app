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
    stage('Deliver') {
        withEnv(['VOLUME=$(pwd)/sources:/src',
        'IMAGE=cdrx/pyinstaller-linux:python2']) {
            sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
            archiveArtifacts 'sources/dist/add2vals'
            sh "ls"
            //sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
        }
    } 
}
