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

//         stage('公共API模块') {
//             steps {
//                 sh 'mvn -f ruoyi-api clean package'
//             }
//         }
        
        stage('公共模块') {
            steps {
                sh 'mvn -f ruoyi-common/ruoyi-common-swagger  clean install -U'
                sh 'echo ------ swagger complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-core clean install -U'
                sh 'echo ------ core complete --------------'
                sh 'mvn -f ruoyi-api/ruoyi-api-system clean install -U'
                sh 'echo ------ system complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-redis clean install -U'
                sh 'echo ------ redis complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-security clean install -U'
                sh 'echo ------ security complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-log clean install -U'
                sh 'echo ------ log complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-datascope clean install -U'
                sh 'echo ------ datascope complete --------------'
                sh 'mvn -f ruoyi-common/ruoyi-common-datasource clean install -U'
                sh 'echo ------ datasource complete --------------'
                
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-swagger -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ swagger complete --------------'
//                 sh 'mvn clean install  -pl ruoyi-common/ruoyi-common-core -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ core complete --------------'
//                 sh 'mvn clean install -pl ruoyi-api/ruoyi-api-system -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ system complete --------------'
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-redis -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ redis complete --------------'
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-security -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ security complete --------------'
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-log -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ log complete --------------'
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-datascope -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ datascope complete --------------'
//                 sh 'mvn clean install -pl ruoyi-common/ruoyi-common-datasource -am -amd -Pdev -Dmaven.test.skip=true'
//                 sh 'echo ------ datasource complete --------------'
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
