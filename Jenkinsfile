#!/usr/bin/env groovy

node{
    def PMD_TOOL='C:/dev/pmd-bin-6.25.0/bin/pmd.bat';
    def CPD_TOOL='C:/dev/pmd-bin-6.25.0/bin/cpd.bat'
    def APEX_RULESET='rulesets/apex/quickstart.xml';
    def PROJECT_DIR='c:/dev/salesforce/demo/force-app/main/default';    
    
    stage('Analyse Code'){
        // Check if folder to store reports exists
        File reportFolder = new File('health-check');
        if(!reportFolder.exists()) { 
            reportFolder.mkdir(); 
        }
      
        // Run Apex PMD code scan
        bat "$PMD_TOOL -d $PROJECT_DIR -R $APEX_RULESET -r health-check/pmd.xml -f  xml -e UTF-8 -failOnViolation false -no-cache";
        
        // Run copy-paste detector
        def cpdOutput = bat "$CPD_TOOL --minimum-tokens 10 --files $PROJECT_DIR/classes --language apex --encoding UTF-8 --format xml --failOnViolation false";
        File cpdOutputFile = new File('health-check/cpd.xml');
        
        if(cpdOutput) {
            cpdOutputFile.write(cpdOutput);
        } else {
            println '*** CPD: No duplications were found'
        }
    }

    stage('Publish Results'){
        // Publish PMD results
        def pmd = scanForIssues tool: pmdParser(pattern: '**/health-check/pmd.xml');
        publishIssues issues: [pmd];

        // Publish CPD results
        def cpd = scanForIssues tool: cpd(pattern: '**/health-check/cpd.xml')
        publishIssues issues: [cpd]
    }
}
