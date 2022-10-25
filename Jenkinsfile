node {
    
    stage ("Checkout Auth Service"){
        git branch: 'main', url: 'https://github.com/abarbuzza/BAH-MCC-Proj-Auth.git'
    }
    
    stage ("Pull Latest Changes") {
        'git pull'
    }
    
    stage ("Gradle") {
        'gradle clean build bootJar'
    }
}
