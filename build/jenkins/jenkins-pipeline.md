# Jenkins Pipeline

The Jenkins pipeline uses a Jenkinsfile managed either through the GUI (and stored in Jenkins) or as a SCM file to coordinate a multi-step build (and potentially deployment) process.

If the Jenkinsfile is managed through the default Jenkins webGUI, you'll need to include a `checkout` step to retrieve the source code from the repo. With a SCM Jenkinsfile, the repo containing the file will be checked out, with checkout options managed through the webGUI.

## Jenkinsfiles
The Jenkinsfiles use a DSL variant of Groovy, and can use either an older 'Scripted' syntax or a newer 'Declarative' syntax.

### Jenkinsfile - Declarative

``` Groovy
pipeline {
    // agent directive is required; this ensures source repo is checked out and made available
    agent any // use any available type of agent/node as executor and workspace for the Pipeline

    // optional `options` block allows setting certain configuration for entire pipeline
    // this can also be declared within a stage for stage-specific options
    options {
        timeout(activity: true, time: 5, unit: 'MINUTES')
        // ...
    }

    stages { // stages directive required

        stage('Build') { // at least one stage required
            steps { // stage must declare steps
                // one or more steps
            }
        }

        stage('Test') {
            steps {
                //
            }
        }

        stage('Deploy') {
            steps {
                //
            }
        }
    }

    // operations to perform after stages
    post {
        always {
            // operations to always perform
        }
        failure {
            // operations to perform only if build failed
        }
        // also supports `unstable`, `success`, and `changed` blocks, and others
    }
}

```

### Groovy Statements and Syntax available in Jenkinsfile

#### Variable Definition
Use `def` to declare a variable:

``` Groovy
def str1 = 'Some text'
def val2 = 5
```

#### Literal Strings
Literal strings can be enclosed in single-quotes, double-quotes, or tripled quotes (double or single).

The `\` character is used to indicate special characters regardless of quote type, so use `\\` for a literal backslash.

#### String Interpolation
Strings enclosed in double-quotes or tripled double-quotes are subject to interpolation:
* `$` operator performs variable interpolation (dot-walking allowed)
* `${}` operator performs statement interpolation; blocks are not allowed but ternary operator is

**Do not use interpolation on sensitive values**: this can expose sensitive variables in the process listings and may potentially mangle them if they contain special characters. Remember that shell statements will have access to the environment. **Guideline**: only use single-quotes around credential values.

``` Groovy
// DO NOT
sh("curl -u ${EXAMPLE_CREDS_USR}:${EXAMPLE_CREDS_PSW} https://example.com/")

// DO
sh('curl -u $EXAMPLE_CREDS_USR:$EXAMPLE_CREDS_PSW https://example.com/')
```

**Beware of using interpolation on user-provided variables**: users can potentially pass special characters into variables which can then inject undesired behavior.


### Useful Steps
* `sh` - run *nix shell commands
* `bat` - run Windows batch commands
* `archiveArtifacts artifacts: glob, fingerprint: bool` - archive built artifacts
* `error [messageString]` - stop the pipeline with a failure
* `powershell` - run a PowerShell script; params:
    - `<script>` (string) - script to run, multiple lines supported
    - `[label]` (string) - displays along with step type
    - `[returnStatus]` (bool) - if true, step return value will be the exit code, automatic fail based on exit code will not occur
    - `[returnStdout]` (bool) - if true, step return value will be the stdout as a string, stdout will not be printed to build log
        - there will always be a trailing space, so .trim() should get called on the output
        - this typically needs to be assigned to a variable (which may require wrapping in a `script` block), and typically requires running the script inside of string interpolation so that it gets converted to a Groovy string:
        ``` Groovy
            script {
                // multi-line string preserves spacing, so we can't indent within it
                mailBody = """
        See <$currentBuild.absoluteUrl>

        ${ powershell(
            label: "read email content",
            returnStdout: true,
            script: '''
            if (Test-Path ".\\email.txt") {
                Get-Content ".\\email.txt"
            }
            '''
        )}"""
            // do stuff with the variable within the script block's scope
            }

        ```
* `script` - enclose arbitrary Groovey script; required for some versions of Jenkins which require any Groovy to be associated with a step
* `tool` - retrieves the location of specified tool installed through jenkins (map to a var or environment var and/or use in script via interpolation)

Script steps support multi-line scripts through triple-quoting (single-quote recommended for most purposes).

Each step gets its own copy of the environment, so persistent modifications of the environment need to be made within the Jenkinsfile Groovy script (e.g. by processing a returnStdout script); these likely do not persist between stages.

### Useful Variables
* `currentBuild.result` - result of current build
    - 'SUCCESS'
    - 'UNSTABLE'

### Useful Directives

#### `agent` - qualify what Agent the pipeline runs on

``` Groovy
agent {
    label nodeLabel // restrict the pipeline to running only on nodes with the specified label
}
```

#### `environment` - modify environment variables

``` Groovy
environment {
    SOME_VARIABLE = 'x'
}
```

Use in `pipeline` block to apply to all steps, use in a `stage` to limit its scope to that stage.

Access within Jenkinsfile via `env.SOME_VARIABLE`.

Jenkins Groovy (or plain Groovy?) requires that environment variables be interpolated prior to string concatenation, e.g. `"${env.SOME_VARIABLE}\\SomePath"` is allowed, while `env.SOME_VARIABLE + "\\SomePath"` is not.

All environment variables are strings due to the constraints of Windows/*nix environment config.

Environment prep configured in the webapp occurs prior to the Jenkinsfile environment setup, so webapp-configured environment can be checked and used as a control on the Jenkinsfile environment.

E.g. for a boolean that defaults to false:

``` Groovy
environment {
    MY_BOOL = "${'true'.equalsIgnoreCase(env.MY_BOOL)}"
}
```

Although it appears that the Groovy for defaulting a string value works (along with the ternary operator), the value restrictions applied to environment variables do not allow for either case to hold a null/empty value.


##### `credentials()` - add credentials to environment

``` Groovy
environment {
    CREDS_VAR = credentials(credentialsIdentifier)
}
```

Sets CREDS_VAR based on the credentials content of the specified identifier. Behavior depends on what type of credentials is identified:
* secret text - the secret text value is is assigned to the environment variable
* username and password - the variable will be set to `username:password` and additional variables will be set:
    + `CREDS_VAR_USR` - the username
    + `CREDS_VAR_PSW` - the password
* secret file - the path to a temporary copy of the secret file

#### `parameters` - access job parameters
Parameters declared in the `parameters` block are accessible to the pipeline. Declared parameters can be accessed in Groovy script as properties of the `params` object, e.g. `params.myParameter`. The webapp parameter prompts are updated to reflect the parameters in the Jenkinsfile as part of running it.

Best practice: transfer parameters into environment variables so that they can be accessed in scripts as well as in Groovy.

Use of parameters in environment variables or within scripts (as opposed to directly from Groovy) will generally rely on string interpolation, so e.g. boolean checks will have to check for "true" or "false" string values.

#### `post`
Run after pipeline or stage is finished (depending on where declared).

Supports `always`, `failure`, `unstable`, `success`, `changed` and other inner blocks to run depending on the pipeline/stage outcome.


#### `timeout` - set a timeout

``` Groovy
timeout(activity: true, time: X, unit: 'SECONDS') {
    // block
}
```

* activity - if set, timeout will be based on log inactivity; else will be absolute
* time - number of units to time
* unit - string from {'DAYS', 'HOURS', 'MINUTES', 'SECONDS', 'MILLISECONDS', 'MICROSECONDS', 'NANOSECONDS'}, default 'MINUTES'

Declare within an `options` block (omitting the block wrapper) to set a timeout for the entire pipeline/stage. Pipeline options timeout appears to have a sporadic issue where it gets treated as absolute for whole job regardless of settings, so this may be more reliable per-stage.

#### `withCredentials` - bind credentials to variables within a block
Use the snippet generator for full options on this. Example:

``` Groovy
    steps {
        withCredentials([usernamePassword(credentialsId: 'GitHubCredentialsId', passwordVariable: 'GitPsw', usernameVariable: 'GitUsr')]) {

        }
    }

    steps {
        // use git credentials within block - Git plugin 4.8.0+
        withCredentials([gitUsernamePassword(credentialsId: 'GitHubCredentialsId')]) {
            powershell '''
            git config credential.helper 'cache'
            git submodule update --init --recursive
            git credential-cache exit
            '''
        }
    }

```


### Useful Script Snippets

#### `node` - restrict where steps will run

``` Groovy
node(nodeName) {
    // some block inside `steps`
}
```


## Auto-Documentation
Built-in auto-generated documentation is available under `${jenkins_url}/pipeline-syntax`. This includes generators for snippets, directives, and documentation links for global variables and other development aspects. This is also linked to from the GUI Jenkinsfile editor.

## Tips
Using the API link at the bottom of a non-pipeline job to view the xml configuration file for the job can provide insights into the steps and credential ids used in the job.

## Sources
* https://www.jenkins.io/doc/book/pipeline/