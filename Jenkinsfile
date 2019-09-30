pipeline {

    // Instructs Jenkins to use the buildconductor docker image to run this pipeline
    
    agent {
        docker { 
            image 'greghodgkinson/jenkins-buildconductor-iib:edge' 
            label 'docker'
            args '-u root'
        }
        
    }

    stages {
    
        // Pull down source code from Git repo
        
       stage('Checkout Code'){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'load']], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Kumaraswamyc/TempConv.git']]])
            }
          
       }   
       
       stage('Checkout .brokerFile'){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'brokerFiles-load']], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Kumaraswamyc/TempConv.git']]])
            }
          
       }  
       
       // Build BAR file from source code
       
        stage('Build BAR') {
            steps {
                sh "/buildconductor/iib/run-automation.sh build tempconvert TemperatureConverter" 
            }
        }   
        
        // Either override and deploy the BAR (with or without broker file), or upload it to UCD so it can be deployed from there (one of the below steps should be uncommented)
        
        stage('Override and Deploy BAR') {
         steps {
                sh "/buildconductor/iib/run-automation.sh overrideAndDeploy tempconvert na na na TEST iibuser iibuser TemperatureConverter TemperatureConverter/overrides/development.properties ../brokerFiles-load/DEV/gregIIBNode.broker"
        
          }
        }
        
        //stage('Override and Deploy BAR') {
        // steps {
        //        sh "/buildconductor/iib/run-automation.sh overrideAndDeploy tempconvert 10.0.10.247 32869 IIB-DEV IIB-DEV iibuser iibuser TemperatureConverter TemperatureConverter/overrides/development.properties"
        //
        //  }
        //}
        
        //stage('Publish to UCD') {	
        //	steps { 
        //	    sh '(mkdir tmp/propertiesCollection && cp load/TemperatureConverter/overrides/* tmp/propertiesCollection && cd tmp && zip ../output/propertiesCollection.zip propertiesCollection/* && cd ..)'

        //	    sh '/buildconductor/iib/run-automation.sh uploadToUcd "Temperature Converter IIB10.0" ${BUILD_NUMBER} output "*.*" admin admin ${BUILD_URL} https://10.0.10.47:8446'            	
        //    }                        	 		
        //}
        
        //stage('Trigger UCD Dev Deploy') {			
         //   steps {			       				 			
         //	      sh '/buildconductor/iib/run-automation.sh requestUcdDeploy "Demo Applications System (BPM, IIB, WDP, WAS)" "Dev" "Deploy Temperature Converter IIB10 (Latest Version)" admin admin ${BUILD_URL} https://10.0.10.47:8446'            			
         //    }                        	 				
         //}

        
    }
}
