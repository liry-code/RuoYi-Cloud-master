pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                expression { changeSet() && currentBuild.changeSets != null && currentBuild.changeSets.size() > 0}
            }
            steps {
                script {
                    def author = getChangeSet()
                    // 使用def变量：注意一定要用双引号，单引号识别为字符串
                    echo "${author}"
                }

                echo '${author}'
                echo 'Deploying'
            }
        }
    }
}


@NonCPS
def getChangeSet(){
    def author = "hhhhh"
    def changeSet = currentBuild.changeSets
    echo "${changeSet}"
    for(int i = 0; i<changeSet.size(); i++){
        def entries = changeSet[i].items
        def entry = entries[0]
        echo  "${entry.author}"
        author += "${entry.author}"
    }

    return author
}


@NonCPS
def getChangeSet(){
    def author = "hhhhh"
    def changeSet = currentBuild.changeSets
    echo "${changeSet}"
    for(int i = 0; i<changeSet.size(); i++){
        def entries = changeSet[i].items
        def entry = entries[0]
        // commit id
        echo  "${entry.commitId}"
        // 时间戳
        echo  "${entry.timestamp}"
        // 提交的信息
        echo  "${entry.msg}"

        // 修改的文章信息List类型
        echo  "${entry.affectedFiles }"
        // 作者
        echo  "${entry.author}"
        author += "${entry.author}"

        def affectedFiles = ew ArrayList(entry.affectedFiles);
        for(int j= 0; j < affectedFiles.size(); j++){
            // 获取受影响的文件信息
            def affectedFile = affectedFiles[i]
            
            // 获取作者对该文件的操作类型
            echo  "${affectedFile.editType.name}"
            // 获取文件路径
            echo  "${affectedFile.path}"
        }
    }

    return author
}
