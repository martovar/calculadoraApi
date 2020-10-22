importar  java.text.SimpleDateFormat
importar  jenkins.model.Jenkins
importar  hudson.model. *
            
nodo ( ' maestro ' ) {
    prueba {
       
        // Herramientas
        env . JAVA_HOME = " $ { herramienta 'openjdk-8' } "
        env . GRADLE = " $ { herramienta 'gradle-6-7' } "        
        env . SONAR_SCANNER = " $ { herramienta 'sonarqube-scanner' } " ;
        
        env . errorEncontrado =  " "
    
        propiedades ([
            buildDiscarder (
                logRotator (
                    numToKeepStr : ' 3 '
                )
            ),
            disableConcurrentBuilds ()            
        ])
        
        stage ( ' Checkout ' ) {
            echo " #################### -> Iniciar pago <- #################### "
            revisa()
            echo " #################### -> Finalizar pago <- #################### "
        }
        
        stage ( ' Limpio ' ) {
            echo " #################### -> Iniciar limpieza <- #################### "
        sh ' chmod + x gradlew '        
            sh ' ./gradlew clean '
            echo " #################### -> Finalizar limpieza <- #################### "
        }
        
        stage ( ' Compilar ' ) {
            echo " #################### -> Iniciar compilación <- #################### "
            sh ' ./gradlew compileJava '
            echo " #################### -> Finalizar compilación <- #################### "
        }
            
        stage ( ' Prueba ' ) {
            echo " #################### -> Iniciar prueba de unidad <- #################### "
            sh ' ./gradlew test '
            junit ' ** / build / test-results / test / *. xml '
            echo " #################### -> Finalizar prueba de unidad <- #################### "
        }

        stage ( ' SonarQube ' ) {
            echo " ################### -> Init SonarQube <- #################### "
            withSonarQubeEnv ( ' sonarqube_local ' ) {
                sh " $ { SONAR_SCANNER } / bin / sonar-scanner -Dproject.settings = sonar-project.properties "
            }
            echo " #################### -> Finalizar SonarQube <- #################### "
        }        
        
        
       stage ( ' Build ' ) {
            echo " #################### -> Iniciar compilación <- #################### "
           sh ' ./gradlew build -x test '
            echo " #################### -> Finalizar compilación <- #################### "
        }        
        
    } atrapar (err) {
        echo " Hubo un error en el pipeline "
        env . errorEncontrado = err . getMessage ()
        echo env . errorEncontrado
        currentBuild . resultado =  ' FALLO '
    }
    
} // nodo de aleta

def  checkout () {
    revisa([
                $ clase : ' GitSCM ' ,
                sucursales : [[
                    nombre : ' * / master '
                ]],
                doGenerateSubmoduleConfigurations : falso ,
                extensiones : [],
                gitTool : ' git ' ,
                submoduleCfg : [],
                userRemoteConfigs : [[
                    credentialsId : ' github_personal ' ,
                    url : ' https://github.com/JULIANCHO923/CalculadoraAPI '
                ]]
            ])
}
