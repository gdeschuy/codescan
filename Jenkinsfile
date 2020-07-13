#!/usr/bin/env groovy

node{
    def PMD_TOOL='C:/dev/pmd-bin-6.25.0/bin/pmd.bat';
    def CPD_TOOL='C:/dev/pmd-bin-6.25.0/bin/cpd.bat'
    def APEX_RULESET='rulesets/apex/quickstart.xml';
    def PROJECT_DIR='c:/dev/salesforce/demo/force-app/main/default';    
    
    stage('Analyse Code'){
        File reportFolder = new File('health-check');
        if(!reportFolder.exists()) { reportFolder.mkdir(); }
      
        bat "$PMD_TOOL -d $PROJECT_DIR -R $APEX_RULESET -r health-check/pmd.xml -f  xml -e UTF-8 -failOnViolation false -no-cache";
        bat "$CPD_TOOL --minimum-tokens 100 --files $PROJECT_DIR/classes --encoding UTF-8 --format xml --failOnViolation false";
    }

    stage('Publish Results'){
        def pmd = scanForIssues tool: pmdParser(pattern: '**/health-check/pmd.xml');
        publishIssues issues: [pmd];

        def cpd = scanForIssues tool: cpd(pattern: '**/health-check/cpd.xml')
        publishIssues issues: [cpd]
    }
}
