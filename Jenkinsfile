node {
    properties([
        pipelineTriggers([
            cron('H/5 * * * *')
        ])
    ])

   def workspace = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\"
    def REPO_URL = "https://github.com/Ayushghagre/clearWorkspace.git" 
    currentBuild.result = "SUCCESS"

    try {
        stage("CHECKOUT")
        {
            checkout scm
        }

        stage("clearing up Workspace") {
            def remoteBranches = bat(script: "git ls-remote --heads ${REPO_URL}", returnStdout: true).trim()

            def branchList = remoteBranches.readLines()
                   .findAll { it.contains('refs/heads/') } 
                   .collect { it.split()[1].replaceAll('refs/heads/', '') } 

           
           def command = "dir /B /A:D ${workspace}"
            
           def workspaceDirs = bat(script: command, returnStdout: true).trim().readLines().drop(1)
            
           workspaceDirs.each { dir ->
            if (!branchList.contains(dir)) {
                echo "Deleting workspace for branch: ${dir}"
                bat "rmdir /S /Q ${workspace}\\${dir}"
            }
        }
          

        }
    } catch (Exception e) {
        echo "Encountered An Exception"
        currentBuild.result = "FAILURE"
        echo e.toString()
    }
} 
