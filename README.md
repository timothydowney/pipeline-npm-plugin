# Pipeline NPM Plugin

This plugin provides integration with Pipeline by configuring an NPM environment to use within a pipeline job by calling `sh npm` or `bat npm`.  This is accomplished by adding an `npmrc` as a custom config file to be centrally managed  by Jenkins.  This is useful for managing registries, authorizations, and any other npm settings that one would like to manage via Jenkins outside of the pipeline itself.

For example:
```
withNPM(npmrcConfig: 'my-custom-nprc') {
    sh 'npm install'
}
```
'my-custom-npmrc' is a config file that has been previously added to Jenkins via Managed Files.  Underneath, the custom `npmrc` file is being copied to the workspace where it will serve a local override for the build.  Nominally, npm would have allowed a command line mechanism to refer to an npmrc file, but for now, it does not appear to.

This plugin requires a local installation of NPM on the agent or may be used via a docker step.

Here's another example using Docker and [Pipeline Model Definition](https://github.com/jenkinsci/pipeline-model-definition-plugin/wiki/getting%20started):

```
stage('npm-build') {
    agent {
        docker {
            image 'node:7.4'
        }
    }

    steps {
        echo "Branch is ${env.BRANCH_NAME}..."

        withNPM(npmrcConfig:'my-custom-npmrc') {
            echo "Performing npm build..."
            sh 'npm install'
        }
    }
}
```

