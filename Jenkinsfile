pipeline {
  agent any
  triggers {
    GenericTrigger(
     token: 'abc',
     genericVariables: [
      [key: 'GITHUB_ACTION',     value: '$.action'],
      [key: 'GITHUB_BASE_SHA',   value: '$.pull_request.base.sha'],
      [key: 'GITHUB_HEAD_SHA',   value: '$.pull_request.head.sha'],
      [key: 'GITHUB_BASE_REF',   value: '$.pull_request.base.ref'],
      [key: 'GITHUB_HEAD_REF',   value: '$.pull_request.head.ref'],
      [key: 'GITHUB_BASE_REPO',  value: '$.pull_request.base.repo.git_url'],
      [key: 'GITHUB_HEAD_REPO',  value: '$.pull_request.head.repo.git_url'],
      [key: 'GITHUB_BASE_NAME',  value: '$.pull_request.base.repo.name'],
      [key: 'GITHUB_BASE_OWNER', value: '$.pull_request.base.repo.owner.login'],      
     ],
    )
  }
  
  stages {
    stage('Print vars') {
      steps {
        sh "echo $GITHUB_ACTION"
        sh "echo $GITHUB_BASE_SHA"
        sh "echo $GITHUB_HEAD_SHA"
        sh "echo $GITHUB_BASE_REF"
        sh "echo $GITHUB_HEAD_REF"
        sh "echo $GITHUB_BASE_REPO"
        sh "echo $GITHUB_HEAD_REPO"
        sh "echo $GITHUB_BASE_NAME"
        sh "echo $GITHUB_BASE_OWNER"
      }
    }
    
    stage('Set initial status') {
        steps {
            script {
                setStatus("pending", "Analyzing!")
            }
        }
    }   
    
    stage('Base checkout') {
        steps {
            dir("${BUILD_TAG}") {
                checkout changelog: false, 
                    poll: false, 
                    scm: scmGit(
                        branches: [[name: 'refs/heads/$GITHUB_BASE_REF']], 
                        extensions: [], 
                        userRemoteConfigs: [
                            [refspec: '+refs/heads/$GITHUB_BASE_REF:branch_to_analyze', 
                            url: 'https://github.com/tntnkn/devopsconf_2024_1']
                        ]
                    )  
            }
        }
    }
  }
}

def setStatus(status, description)
{
    sh """
    curl -L \
        -X POST \
        -H "Accept: application/vnd.github+json" \
        -H "Authorization: Bearer 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" \
        -H "X-GitHub-Api-Version: 2022-11-28" \
        https://api.github.com/repos/$GITHUB_BASE_OWNER/$GITHUB_BASE_NAME/statuses/$GITHUB_HEAD_SHA \
        -d '{"state":"${status}","description":"${description}","context":"Analyze"}'
    """
}
