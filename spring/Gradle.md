## Spring Cloud

*gradle.build*

    
    buildscript {
    
    repositories {
    mavenCentral()
    }
    dependencies {
    classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.2.RELEASE")
    }
    }
    
    apply plugin: 'java'
    apply plugin: 'eclipse'
    apply plugin: 'idea'
    apply plugin: 'org.springframework.boot'
    
    
    jar {
    baseName = 'gs-rest-service'
    version =  '0.1.0'
    }
    
    repositories {
    mavenCentral()
    }
    
    sourceCompatibility = 1.8
    targetCompatibility = 1.8
    
    dependencyManagement {
      imports {
    mavenBom 'org.springframework.cloud:spring-cloud-dependencies:Camden.SR6'
      }
    }
    
    dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    compile("org.springframework.cloud:spring-cloud-starter-eureka-server")
    testCompile('org.springframework.boot:spring-boot-starter-test')
    }
