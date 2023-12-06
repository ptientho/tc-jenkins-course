pipeline {
    agent any 
    stages {
        stage('Print and list current directory') {
            steps {
                sh 'pwd'
                sh 'ls -al'
                sh 'echo "Hi ${NAME}!"'
            }
        }
        stage('Show ROS environment variables') {
            steps {
                sh 'env | grep ROS'
            }
        }
        stage('Move the robot') {
            steps {
                sh '''
                roslaunch publisher_example move.launch &
                MOVE_ID=$!
                sleep ${MOVE}s
                kill $MOVE_ID
                '''
            }
        }
        stage('Stop the robot') {
            steps {
                sh '''
                roslaunch publisher_example stop.launch &
                STOP_ID=$!
                sleep 5s
                kill $STOP_ID
                '''
            }
        }
        stage('Reset the simulation') {
            steps {
                sh '''
                case $RESET_SIMULATION in
                  true)    
                    rosservice call /gazebo/reset_simulation "{}"
                    echo "Simulation has been reset."
                    ;;
                  false)   
                    echo "Not resetting the simulation."
                    ;;
                esac
                '''
            }
        }
        stage('Done') {
            steps {
                sleep 5
                echo 'Pipeline completed'
            }
        }
    }
}