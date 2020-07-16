#!/usr/bin/env groovy

/**
* Code scanning build job to be run on Windows machines against Salesforce source code.
* The Jenkins Warnings Next Generation Plugin should be installed together with PMD and CPD.
*
* @author  Glenn Deschuymer
* @version 1.0
* @since   13-07-2020
*/

node{
    def PMD_TOOL='C:/dev/pmd-bin-6.25.0/bin/pmd.bat';
    def CPD_TOOL='C:/dev/pmd-bin-6.25.0/bin/cpd.bat';
    def APEX_RULESET='rulesets/apex/quickstart.xml';    
    def SF_APEX_DOCS = 'c:/dev/sfapexdoc/SfApexDoc.jar';

    def PROJECT_DIR=${env.PROJECT_DIR};

    stage('Prepare build'){
        File reportFolder = new File('health-check');        
        if(!reportFolder.exists()) { 
            reportFolder.mkdir(); 
        }
    }

    stage('PMD'){
        bat(returnStdout: false, script: "$PMD_TOOL -d $PROJECT_DIR -R $APEX_RULESET -r health-check/pmd.xml -f  xml -e UTF-8 -failOnViolation false -no-cache");
        def pmd = scanForIssues tool: pmdParser(pattern: '**/health-check/pmd.xml');
        publishIssues issues: [pmd];
    }

    stage('CPD'){
        def cpdOutput = bat(returnStdout: true, script: "@$CPD_TOOL --minimum-tokens 10 --files $PROJECT_DIR/classes --language apex --encoding UTF-8 --format xml --failOnViolation false");        
        
        if(cpdOutput) {
            File cpdOutputFile = new File(pwd()+"/health-check/cpd.xml");
            cpdOutputFile.write(cpdOutput);            
        } else {
            println("CPD: No duplications found");
        }

        def cpd = scanForIssues tool: cpd(pattern: '**/health-check/cpd.xml');
        publishIssues issues: [cpd];
    }

    stage('Javadoc'){
        bat "java -jar $SF_APEX_DOCS -s $PROJECT_DIR/classes";
        step([$class: 'JavadocArchiver', javadocDir: './SfApexDocs', keepAll: 'true']);
    }
}