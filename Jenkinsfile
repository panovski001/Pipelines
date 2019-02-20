pipeline{
    agent any
    environment{
        name = 'Snaplogic intergration'
        SNAPLEX = 'https://elastic.snaplogic.com'
        prj_name = 'IlijaPanovski'
        prj_name_m = 'ilija_2'
        prj_location = 'Interworks-Partner/projects'
        operation = 'migrate'
		organization = 'Interworks-Partner'
		GITHUB = credentials('githubuserpass')
    }
    stages{
		stage('Git Checkout'){
            steps{
               checkout([$class: 'GitSCM', 
			   branches: [[name: '*/master']], 
			   doGenerateSubmoduleConfigurations: false, 
			 /*  extensions: [[$class: 'PerBuildTag']], */
			   submoduleCfg: [], 
			   userRemoteConfigs: [[credentialsId: 'ilijagithub', 
			   url: 'git@github.com:panovski001/Pipelines.git']]])
            }
        }
        stage('Snaplogic Activity'){
            steps{
				sh """
				    if [ -d "out" ];then
						exit 0
					 else
						mkdir out 
				    fi
				   """
                httpRequest acceptType: 'APPLICATION_JSON',
				authentication: 'iwsnaplogic', 
                contentType: 'APPLICATION_JSON', 
                httpMode: 'GET', 
				outputFile: 'out/SnaplexStatusActivity.txt',
                ignoreSslErrors: true, 
                requestBody: "",
                responseHandle: 'NONE', 
                url: "${SNAPLEX}/api/1/rest/public/activities/Interworks-Partner"
            }
        }
		stage('Snaplogic Deploy'){
            steps{
                httpRequest authentication: 'iwsnaplogic', 
                contentType: 'APPLICATION_JSON', 
                httpMode: 'GET', 
                ignoreSslErrors: true, 
                requestBody: '',
				outputFile: 'out/IlijaPlex.txt',
                responseHandle: 'NONE', 
                url: "${SNAPLEX}/api/1/rest/public/snaplex/Interworks-Partner?plex_path=/Interworks-Partner/projects/IlijaPanovski/Ilijaplex"
            }
        }
		stage('Git Tag'){
            steps{
                sh """
		   git remote set-url origin git@github.com:panovski001/Pipelines.git
                   git tag -a $BUILD_TAG -m "$BUILD_TAG"
                   git push --tag
		   """
            }
        }
    }
}
