pipeline {
    agent any

    environment {
         CHECKMARX_CREDENTIALS_ID = 'SASTCred'
         CHECKMARX_USERNAME = credentials('CxSASTUser')
         CHECKMARX_PASSWORD = credentials('CxSASTPassword')
        // CHECKMARX_PROJECT_NAME = credentials('CxProject')
        CHECKMARX_SERVER_URL = 'http://10.33.0.67'
        CHECKMARX_SCA = credentials('CxSCAUser')
        CHECKMARX_SCAPASS = credentials('CxSASTPassword')
        PROJECT_NAME = 'SastScaPipelineshubh'
        CHECKMARX_SCA_SERVER_URL = 'https://api-sca.checkmarx.net'
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    echo 'Preparing workspace and setting up environment.'
                }
            }
        }
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    //userRemoteConfigs: [[url: 'https://github.com/vbarhate/JavaVulnerableLabE.git']]
                    userRemoteConfigs: [[url: 'https://github.com/SonarSource/sonar-plugin-api.git']]
                         
                ])
            }
        }
        stage('Checkmarx SAST Scan') {
            steps {
                script {
                    step([
                        $class: 'CxScanBuilder',
                        comment: '',
                        configAsCode: true,
                        credentialsId: env.CHECKMARX_CREDENTIALS_ID,
                        customFields: '',
                        excludeFolders: '',
                        exclusionsSetting: 'global',
                        failBuildOnNewResults: false,
                        failBuildOnNewSeverity: 'CRITICAL',
                        filterPattern: '''
                            !**/_cvs/**/*, !**/.svn/**/*, !**/.hg/**/*, !**/.git/**/*, !**/.bzr/**/*,
                            !**/.gitgnore/**/*, !**/.gradle/**/*, !**/.checkstyle/**/*, !**/.classpath/**/*, !**/bin/**/*,
                            !**/obj/**/*, !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr, !**/*.iws,
                            !**/*.bak, !**/*.tmp, !**/*.aac, !**/*.aif, !**/*.iff, !**/*.m3u, !**/*.mid, !**/*.mp3,
                            !**/*.mpa, !**/*.ra, !**/*.wav, !**/*.wma, !**/*.3g2, !**/*.3gp, !**/*.asf, !**/*.asx,
                            !**/*.avi, !**/*.flv, !**/*.mov, !**/*.mp4, !**/*.mpg, !**/*.rm, !**/*.swf, !**/*.vob,
                            !**/*.wmv, !**/*.bmp, !**/*.gif, !**/*.jpg, !**/*.png, !**/*.psd, !**/*.tif, !**/*.swf,
                            !**/*.jar, !**/*.zip, !**/*.rar, !**/*.exe, !**/*.dll, !**/*.pdb, !**/*.7z, !**/*.gz,
                            !**/*.tar.gz, !**/*.tar, !**/*.gz, !**/*.ahtm, !**/*.ahtml, !**/*.fhtml, !**/*.hdm,
                            !**/*.hdml, !**/*.hsql, !**/*.ht, !**/*.hta, !**/*.htc, !**/*.htd, !**/*.war, !**/*.ear,
                            !**/*.htmls, !**/*.ihtml, !**/*.mht, !**/*.mhtm, !**/*.mhtml, !**/*.ssi, !**/*.stm,
                            !**/*.bin,!**/*.lock,!**/*.svg,!**/*.obj,
                            !**/*.stml, !**/*.ttml, !**/*.txn, !**/*.xhtm, !**/*.xhtml, !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*,
                            !OSADependencies.json, !**/node_modules/**/*, !**/.cxsca-results.json, !**/.cxsca-sast-results.json, !.checkmarx/cx.config
                        ''',
                        fullScanCycle: 10,
                        groupId: '1',
                        password: env.CHECKMARX_PASSWORD,
                        preset: '36',
                        projectLevelCustomFields: '',
                        projectName: 'JavaVulnerableLabE-shubh',
                        sastEnabled: true,
                        serverUrl: env.CHECKMARX_SERVER_URL,
                        sourceEncoding: '1',
                        username: env.CHECKMARX_USERNAME,
                        vulnerabilityThresholdResult: 'FAILURE',
                        generatePdfReport: true,
                        waitForResultsEnabled: true
                    ])
                }
            }
        }
        stage('Checkmarx SCA Scan') {
            steps {
                echo 'Running Checkmarx security scan...'
                step([
                $class: 'CxScanBuilder',
                comment: 'Security Scan',
                configAsCode: true,
                credentialsId: 'CxSCA',
                customFields: '',
                dependencyScanConfig: [
                    dependencyScanExcludeFolders: '',
                    dependencyScanPatterns: '',
                    dependencyScannerType: 'SCA',
                    enableScaResolver: 'SCA_RESOLVER',
                    fsaVariables: '',
                    osaArchiveIncludePatterns: '*.zip, *.war, *.ear, *.tgz',
                    overrideGlobalConfig: true,
                    pathToScaResolver: 'C:\\Plugins\\Jenkins\\Novartis\\ScaResolver-win64',
                    scaAccessControlUrl: 'https://platform.checkmarx.net',
                    scaCredentialsId: 'CxSCA',
                    scaServerUrl: env.CHECKMARX_SCA_SERVER_URL,
                    scaTeamPath: '/CxServer/Plugins',
                    scaTenant: 'Plugins',
                    scaWebAppUrl: 'https://sca.checkmarx.net',
                    scaResolverAddParameters: '--log-level debug'
                ],
                excludeFolders: '',
                exclusionsSetting: 'global',
                failBuildOnNewResults: false,
                failBuildOnNewSeverity: 'CRITICAL',
                filterPattern: '''
                    !**/_cvs/**/*, !**/.svn/**/*, !**/.hg/**/*, !**/.git/**/*, !**/.bzr/**/*,
                    !**/.gitgnore/**/*, !**/.gradle/**/*, !**/.checkstyle/**/*, !**/.classpath/**/*, !**/bin/**/*,
                    !**/obj/**/*, !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr, !**/*.iws,
                    !**/*.bak, !**/*.tmp, !**/*.aac, !**/*.aif, !**/*.iff, !**/*.m3u, !**/*.mid, !**/*.mp3,
                    !**/*.mpa, !**/*.ra, !**/*.wav, !**/*.wma, !**/*.3g2, !**/*.3gp, !**/*.asf, !**/*.asx,
                    !**/*.avi, !**/*.flv, !**/*.mov, !**/*.mp4, !**/*.mpg, !**/*.rm, !**/*.swf, !**/*.vob,
                    !**/*.wmv, !**/*.bmp, !**/*.gif, !**/*.jpg, !**/*.png, !**/*.psd, !**/*.tif, !**/*.swf,
                    !**/*.jar, !**/*.zip, !**/*.rar, !**/*.exe, !**/*.dll, !**/*.pdb, !**/*.7z, !**/*.gz,
                    !**/*.tar.gz, !**/*.tar, !**/*.gz, !**/*.ahtm, !**/*.ahtml, !**/*.fhtml, !**/*.hdm,
                    !**/*.hdml, !**/*.hsql, !**/*.ht, !**/*.hta, !**/*.htc, !**/*.htd, !**/*.war, !**/*.ear,
                    !**/*.htmls, !**/*.ihtml, !**/*.mht, !**/*.mhtm, !**/*.mhtml, !**/*.ssi, !**/*.stm,
                    !**/*.bin,!**/*.lock,!**/*.svg,!**/*.obj,
                    !**/*.stml, !**/*.ttml, !**/*.txn, !**/*.xhtm, !**/*.xhtml, !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*,
                    !OSADependencies.json, !**/node_modules/**/*, !**/.cxsca-results.json, !**/.cxsca-sast-results.json, !.checkmarx/cx.config
                ''',
                fullScanCycle: 10,
                generateScaReport: true,
                scaReportFormat: 'PDF',
                projectName: env.PROJECT_NAME,
                serverUrl: env.CHECKMARX_SERVER_URL,
                sastEnabled: false,
                vulnerabilityThresholdResult: 'FAILURE',
                waitForResultsEnabled: true
                ])
            }
        }
     }

    post {
        success {
        echo 'Checkmarx scan completed successfully!'
        }
        failure {
        echo 'Checkmarx scan failed!'
        }
    }
}
