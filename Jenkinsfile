#!groovy

@Library('github.com/mozmeao/jenkins-pipeline@20170315.1')

def loadBranch(String branch) {
    // load the utility functions used below
    utils = load 'jenkins/utils.groovy'
    // defined in the Library loaded above
    setGitEnvironmentVariables()
    parts = branch.split('/')
    if ( parts[0] == 'fast' ) {
        branch = parts[1]
        env.BRANCH_NAME = branch
        parallel = true
    } else {
        parallel = false
    }
    if ( fileExists("./jenkins/branches/${branch}.yml") ) {
        config = readYaml file: "./jenkins/branches/${branch}.yml"
        if ( parallel ) {
            config.parallel = parallel
        }
        regions = readYaml file: 'jenkins/regions.yml'
        default_script = parallel ? 'configure-parallel' : 'configure'
        config_script = config.script ? "./jenkins/${config.script}.groovy" : "./jenkins/${default_script}.groovy"
        println "Loading ${config_script}"
        load config_script
    } else {
        println "No config for ${branch}. Nothing to do. Good bye."
        return
    }
}

node {
    stage ('Prepare') {
        checkout scm
        stash 'workspace'
    }
    loadBranch(env.BRANCH_NAME)
}
