node {
    
    stage ("Checkout React Client"){
        git branch: 'main', url: 'https://github.com/foxwas/bahmsdfox-reactclient.git'
    }
    
    stage ("Install dependencies - React Client") {
        bat 'npm install'
    }
    
    stage ("Launch - React Client") {
        bat 'npm start'
    }
}
    