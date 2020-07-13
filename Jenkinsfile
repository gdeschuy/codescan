#!/usr/bin/env groovy

node{
    def PMD_TOOL='C:/dev/pmd-bin-6.25.0/bin/pmd.bat';
    def APEX_RULESET='rulesets/apex/quickstart.xml';
    def PROJECT_DIR='c:/dev/salesforce/demo/force-app/main/default';    
    
    stage('Analyse Code'){
        def reportFolder = new File('health-check');

        if(!reportFolder.exists()){
            println("Folder does not exist")       
            reportFolder.mkdir();
        } else{
            println("Folder exists")
        }
      
        //bat "$PMD_TOOL -d $PROJECT_DIR -R $APEX_RULESET -r health-check/pmd.xml -f  xml -e UTF-8 -failOnViolation false -no-cache";
    }

    stage('Publish Results'){
        //def pmd = scanForIssues tool: pmdParser(pattern: '**/health-check/pmd.xml');
        //publishIssues issues: [pmd];
    }
}
