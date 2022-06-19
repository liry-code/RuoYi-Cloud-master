pipeline {
    agent any
    tools {
        maven "maven3.8.5"
    }

    stages {
//         stage('全局打包') {
//             steps {
//                 sh 'mvn -B -DskipTests clean package'
//             }
//         }

        stage('公共API模块') {
            steps {
                sh 'mvn clean install -pl ruoyi-api -am -U'
                sh 'echo ------ ruoyi-api complete --------------'
                
                sh 'mvn clean install -pl ruoyi-api/ruoyi-api-system -am -U'
                sh 'echo ------ ruoyi-api-system complete --------------'
            }
        }
        
        stage('公共模块') {
            steps {
               
                sh 'mvn clean install  -pl ruoyi-common -U'
                sh 'echo ------ ruoyi-common complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-swagger -U'
                sh 'echo ------ ruoyi-common-swagger complete --------------'
                 
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-datasource -U'
                sh 'echo ------ ruoyi-common-datasource complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-core -U'
                sh 'echo ------ ruoyi-common-core complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-redis -U'
                sh 'echo ------ ruoyi-common-redis complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-security -U'
                sh 'echo ------ ruoyi-common-security complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-log -U'
                sh 'echo ------ ruoyi-common-log complete --------------'
                
                sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-datascope -U'
                sh 'echo ------ ruoyi-common-datascope complete --------------'
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
    def affectedFileList[]
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
            affectedFileList[j] = ${affectedFile.path}
            
        }
    }
    echo "${affectedFileList}"

    return author
}
