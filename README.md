## Summary

Openshift Jenkins agent with Python 3.6.
Built from quay.io/openshift/origin-jenkins-agent-base

## Example pipeline
```
pipeline{
  agent {
    node {
      label 'python'
    }
  }
  environment {
      GIT_REPO="${SOURCE_REPOSITORY_URL}"
      GIT_SSL_NO_VERIFY="true"
      GIT_BRANCH="${SOURCE_REPOSITORY_REF}"
      BUILD_CONFIG="${NAME}"
  }
  stages {
    stage('Get Latest Code') {
      steps {
        git branch: "${GIT_BRANCH}", url: "${GIT_REPO}", credentialsId: "scm-credentials" // declared in environment
      }
    }
    stage("Install build requirements") {
      steps {
        // Archive the build output artifacts.
        sh """
        scl enable rh-python36 'virtualenv --no-site-packages .'
        source bin/activate
        pip3 install --upgrade pip
        pip3 install mkdocs
        deactivate
        """
      }
    }
    stage("Build the docs") {
      steps {
        sh '''
        source bin/activate
        mkdocs build
        deactivate
        '''
      }
    }
    stage("Trigger build"){
      steps {
        sh 'oc start-build ${BUILD_CONFIG} --from-dir=site --follow'
      }
    }
  }
}
```

## Building on OpenShift
### Python 3.6
```
oc new-build https://github.com/tomasliumparas/container-jenkins-slave-python.git \
  --name jenkins-slave-python-36 \
  --context-dir="3.6"
```
