def gv
pipeline{
  
  agent any
  parameters{
    // string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
    choice(name: 'VERSION', choices:['1.1.0','1.2.0','1.3.0'], description:'')
    booleanParam(name:'executeTests',defaultValue: true, description: '')
  }
  // tools{
  //   maven : "maven-3.8.6"
  // }
  environment{
    NEW_VERSION = '1.3.0'
    SERVER_CREDENTIALS = credentials('demo-server-cred')
  }
  stages {
    stage("init"){
      steps{ 
        script{
          gv = load "script.groovy"
        }
      }
    }

    stage("build"){
      steps{ 
        script{
          gv.buildApp();
        }
      }
    }
    
    stage("test"){
      // when{
      //   expression{
      //     env.BRANCH_NAME == 'dev' || env.BRANCH_NAME == 'master'
      //   }
      // }
      when{
        expression{
          params.executeTests == true
        }
      }
      steps{
        script{
          gv.testApp()
        }
      }
    }
    
    stage("deploy"){
      input{
        message "Select the environment to deploy to"
        ok "Env Selected"
        parameters{
          choice(name: 'ENV', choices:['dev','stage','prd'], description:'')
        }
      }
      steps{
        script{
          gv.deployApp()
          echo "Deploying to {$ENV}"
        }
        // withCredentials([
        //   usernamePassword(credentials:'demo-server-cred',usernameVariable: USER, passwordVariable:PWD)
        // ]){
        //   echo "${USER} ${PWD}"   
        // }
      }
    }
  }
//   post{
//     always{
//       echo 'email sent'
//     }
//     success{
      
//     }
//     failure{
      
//     }
//   }
}
