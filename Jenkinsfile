#!/usr/bin/env groovy

node{
    stage('ApexPMD'){
        bat(script:"'C:\Program Files (x86)\pmd-bin-6.25.0\bin\pmd.bat' -d 'c:\dev\jenkins\demo\force-app\main\default' -R rulesets/apex/quickstart.xml -e UTF-8 -f xml --no-cache");
    }
}
