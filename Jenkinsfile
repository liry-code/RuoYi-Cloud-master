pipeline {
    agent any
    tools {
        maven "maven3.8.5"
    }

    stages {
        stage('全局打包') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

//         stage('公共API模块') {
//             steps {
//                 sh 'mvn -f ruoyi-api clean package'
//             }
//         }
        
        stage('公共模块') {
            steps {
                sh 'mvn -f ruoyi-common/ruoyi-common-swagger  clean install'
                sh 'echo ------ swagger complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-core clean install'
                sh 'echo ------ core complete --------------'
                sh 'mvn -f ruoyi-api/ruoyi-api-system clean install'
                sh 'echo ------ system complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-redis clean install'
                sh 'echo ------ redis complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-security clean install'
                sh 'echo ------ security complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-log clean install'
                sh 'echo ------ log complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-datascope clean install'
                sh 'echo ------ datascope complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-datasource clean install'
                sh 'echo ------ datasource complete --------------'
            }
        }
        
        
        stage('微服务模块') {
            steps {
                sh 'mvn -f ${project_name} clean package'
            }
        }
        
        stage('复制文件') {
            steps {
                sh 'echo -----waiting-------'
            }
        }
        
        stage('Example Deploy') {
            when {
                expression { currentBuild.changeSets != null && currentBuild.changeSets.size() > 0}
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

        def affectedFiles = new ArrayList(entry.affectedFiles);
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
