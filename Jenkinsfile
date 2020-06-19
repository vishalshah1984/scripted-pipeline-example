// #!groovy

def codeCheckout() 
{
    cleanWs()
    checkout scm: [$class: 'GitSCM', branches: [[name: 'master']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: "maven-samples"]], userRemoteConfigs: [[url: "https://github.com/gabrielf/maven-samples.git"]]]
}

def build()
{
    sh"mvn clean install"
}


def theme(buildStatus,buildTheme)
{
    if(buildTheme == "Start")
    {
      ansiColor('xterm') 
      {
      echo "\033[1m\033[34m${buildStatus} \033[0m"
      }
    }
    else
    {
      ansiColor('xterm') 
      {
      echo "\033[1m\033[32m${buildStatus} \033[0m"
      }
      }
}

def start(buildStatus)
{
    buildTheme = "Start"
    theme(buildStatus,buildTheme)
}


def complete(buildStatus)
{
    buildTheme = "Completed"
    theme(buildStatus,buildTheme)
}

node('master') 
{
  timeout(time: 1, unit: 'HOURS')
  tool name: 'maven-latest', type: 'maven'
  {
  try {
         stage ("Code Checkout") 
         {
              buildStatus = "Code Checkout Started"
              start(buildStatus)
              codeCheckout()
              buildStatus = "Code Checkout Completed"
              complete(buildStatus)
         }
      } 
      catch (err) 
      {
            status = "Code Checkout Failed!"
            error("Error in Code Checkout!")
      }

  try {
         stage ("Build") 
         {
              buildStatus = "Build Promotion Started"
              start(buildStatus)
              build()
              buildStatus = "Build Promotion Completed"
              complete(buildStatus)
         }
      } 
      catch (err) 
      {
            error("Error in Build Promotion!")
      }
  }
}