node() {
    stage('Build') {
        docker.image('python:3.10.1-alpine').inside {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    try {
        stage('Test') {
            docker.image('qnib/pytest').inside {
                checkout scm
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } 
        }
    } finally {
        junit 'test-reports/results.xml'
    }
    try {
        stage('Deliver') {
            docker.image('cdrx/pyinstaller-linux:python2').inside {
                checkout scm
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
        } 
    } finally {
        'dist/add2vals'
    }
}
