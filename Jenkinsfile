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
    }
    stages{
		stage('Git Checkout from tag'){
            steps{
                sshagent(['ilijagithub']) {
                    sh """
		    	git remote set-url origin git@github.com:panovski001/Pipelines.git
                        git checkout $tag
		               """
                }
            }
        }
        stage('Snaplogic Activity'){
            steps{
				sh """
				    if [ -d "out" ];then
						mv out 
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
                sshagent(['ilijagithub']) {
                    sh """
                        git tag -a $BUILD_TAG -m "$BUILD_TAG"
                        git push --tag
		               """
                }
            }
        }
    }
}
