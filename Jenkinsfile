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
            when{
                expression {
                    isContainModule("ruoyi-api") || isContainModule("ruoyi-common")
                }
            }
            steps {
                
                sh 'mvn clean install -pl ruoyi-api -am -U'
                sh 'echo ------ ruoyi-api complete --------------'
                
                sh 'mvn clean install -pl ruoyi-api/ruoyi-api-system -am -U'
                sh 'echo ------ ruoyi-api-system complete --------------'
            }
        }
        
        stage('公共模块') {
            when{
                expression {
                    isContainModule("ruoyi-api") || isContainModule("ruoyi-common")
                }
            }
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
        
        
        stage('认证模块') {
            when {
                expression  {
                    isContainModule("ruoyi-auth")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-auth complete --------------'
            }
        }
        
        stage('网关模块') {
            when {
                expression  {
                    isContainModule("ruoyi-gateway")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-gateway complete --------------'
            }
        }
        
        stage('文件模块') {
            when {
                expression  {
                    isContainModule("ruoyi-modules/ruoyi-file")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-file complete --------------'
            }
        }
        
        stage('代码生成模块') {
            when{
                expression  {
                    isContainModule("ruoyi-modules/ruoyi-gen")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-gen complete --------------'
            }
        }
        
        stage('定时任务模块') {
            when{
                expression  {
                    isContainModule("ruoyi-modules/ruoyi-job")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-job complete --------------'
            }
        }
        
        stage('系统模块') {
            when{
                expression  {
                    isContainModule("ruoyi-modules/ruoyi-system")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-system complete --------------'
            }
        }
        
        stage('监控模块') {
            when{
                expression  {
                    isContainModule("ruoyi-modules/ruoyi-monitor")
                }
            }
            steps {
                sh 'mvn -f ${project_name} clean package'
                sh 'echo ------ ruoyi-monitor complete --------------'
            }
        }
        
        
        
        stage('构建镜像') {
            steps {
                sh 'echo -----waiting-------'
            }
        }
        
        stage('镜像部署') {
            when {
                expression { currentBuild.changeSets != null && currentBuild.changeSets.size() > 0}
            }
            steps {
//                 script {
//                     def author = getChangeSet()
//                     // 使用def变量：注意一定要用双引号，单引号识别为字符串
//                     echo "${author}"
//                 }
// 
//                 echo '${author}'
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
    def affectedFileList = []
    for(int i = 0; i < changeSet.size(); i++){
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
            affectedFileList += "${affectedFile.path}"
            
        }
    }
    echo "${affectedFileList}"

    return affectedFileList
}

@NonCPS
def boolean isContainModule(String moduleName){
    def affectedFiles = getChangeSet()
    for(int i = 0 ; i < affectedFiles.size(); i++){
        def filePath = affectedFiles[i]
        if (filePath.startsWith(moduleName)) {
            return true
        }
    }
    return false
}


// 测试
@NonCPS
def boolean isContainModuleTest(String moduleName){
    def affectedFilePathList = ['ruoyi-common/ruoyi-common-log/src/java', 'ruoyi-modules/ruoyi-file/src/java']
    for(int i = 0 ; i < affectedFilePathList.size(); i++){
        def filePath = affectedFilePathList[i]
        if (filePath.startsWith(moduleName)){
            return true
        }
    }
    return false
}
