pipeline {
    agent any 
        stages {
          stage ("auth-Build") {
              steps {
                  sh 'docker build -t image1 ./auth '
              }
          }
          stage ('auth-configure') {
              steps {
                  sh 'docker run -itd --name auth_service -p 5001:5001 image1'
              }
          }
           stage ("book-Build") {
              steps {
                  sh 'docker build -t image2 ./book '
              }
          }
          stage ('book-configure') {
              steps {
                  sh 'docker run -itd --name book_service -p 5002:5002 image2'
              }
          }
           stage ("borrow-Build") {
              steps {
                  sh 'docker build -t image3 ./borrow '
              }
          }
          stage ('borrow-configure') {
              steps {
                  sh 'docker run -itd --name borrow_serivce -p 5003:5003 image3'
              }
          }
    }
}
