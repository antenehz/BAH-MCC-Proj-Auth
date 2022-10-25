node {
    
    stage ("Checkout Auth Service"){
        git branch: 'main', url: 'https://github.com/abarbuzza/BAH-MCC-Proj-Auth.git'
    }
    
    stage ("Gradle") {
        sh 'gradle clean build bootJar'
    }
    
    stage('User Acceptance Test') {
        def response= input message: 'Is this build good to go?',
        parameters: [choice(choices: 'Yes\nNo',
        description: '', name: 'Pass')]
        if(response=="Yes") {
            stage('Deploy') {
                sh 'gradle build -x test'
            }
        }
    }
}
