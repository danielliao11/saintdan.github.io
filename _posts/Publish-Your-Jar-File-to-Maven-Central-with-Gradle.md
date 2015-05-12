title: "Publish Your Jar File to Maven Central with Gradle"
date: 2015-04-16 15:50:45
categories: [Java, Gradle, Maven]
tags: [Maven Central, tutorial]

---
This is a tutorial to help you to publish your jar file to Maven Central with Gradle.

**First of all, we need:**

- Sigh up for Sonatype.  
- Create a JIRA ticket.
- Your own gpg key.

<!-- more -->

##Register your project at Sonatype
After sighing up and creating your own account, click the Create button to create a new issue. Choose the options like this:

- Project: Community Support - Open Source Project Repository Hosting  
- Issue Type: New Project 
 
**NOTICE**: 

- When you create your project, pay more attenton to **groupId**. It is very important.  
**If you don't own a domain, you can choose a groupId like "com.github.your_account" which reflects your project host.** 
- You can't use your JIRA ticket immediately.Sonatype have to prepare repository and normally it takes 1-2 business days.
   
##Generate your own gpg key
Before you publish your **release** version of jar file, you need to sign it with GnuPG.

###Check it with:

```
$ gpg --version
```

###Generate the key with:

```
$ gpg --gen-key
```

You will then be prompted back with the following:

```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection?
```
You will want to select number 1, as it can be used for encryption and decryption, whereas the second and third choices are only allowed to sign messages. To do so, press the number 1, and then press enter.  
You then will be prompted with the following:

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
```

You will want to enter "2048" here, as recommended by gnupg.  
You will be prompted with the following:

```
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
```

If you don't want your key to expire (for the next prompt, select 0).  
Answer yes if the information is correct, when prompted, and then enter your Real Name, Email address, and a comment (which is optional). If everything is correct, press "o" (for Ok) and then enter.  
After entering your passphrase, follow the instructions in the terminal:

```
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
```

You can just wait for a while, or you can type random words, move your mouse to give the gpg enough entropy.  
Your KEY-ID would be the two keys (both identical) above which are in bold. And you can type:

```
$ gpg --list-keys
```

in terminal to list them all.

###Upload your public key to key server:

```
$ gpg --keyserver hkp://pool.sks-keyservers.net --send-keys <your_key_ID>
```

##Gradle Script
I used [Chris Banes](https://plus.google.com/+ChrisBanes/posts?partnerid=ogpy0)'s [gradle-mvn-push](https://github.com/chrisbanes/gradle-mvn-push) and modify it for jar to my [gradle_mvn_push_for_jar](https://github.com/saintdan/gradle_mvn_push_for_jar). THX Chris!

###Create project root maven-push.gradle
You can download my [gradle_mvn_push_for_jar](https://github.com/saintdan/gradle_mvn_push_for_jar) or copy following with no modification:

```
apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'maven'
apply plugin: 'signing'

def isReleaseBuild() {
    return VERSION_NAME.contains("SNAPSHOT") == false
}

def getReleaseRepositoryUrl() {
    return hasProperty('RELEASE_REPOSITORY_URL') ? RELEASE_REPOSITORY_URL
            : "https://oss.sonatype.org/service/local/staging/deploy/maven2/"
}

def getSnapshotRepositoryUrl() {
    return hasProperty('SNAPSHOT_REPOSITORY_URL') ? SNAPSHOT_REPOSITORY_URL
            : "https://oss.sonatype.org/content/repositories/snapshots/"
}

def getRepositoryUsername() {
    return hasProperty('NEXUS_USERNAME') ? NEXUS_USERNAME : ""
}

def getRepositoryPassword() {
    return hasProperty('NEXUS_PASSWORD') ? NEXUS_PASSWORD : ""
}

uploadArchives {
    repositories {
        mavenDeployer {
            beforeDeployment { MavenDeployment deployment -> signing.signPom(deployment) }

            pom.groupId = GROUP
            pom.artifactId = POM_ARTIFACT_ID
            pom.version = VERSION_NAME

            repository(url: getReleaseRepositoryUrl()) {
                authentication(userName: getRepositoryUsername(), password: getRepositoryPassword())
            }
            snapshotRepository(url: getSnapshotRepositoryUrl()) {
                authentication(userName: getRepositoryUsername(), password: getRepositoryPassword())
            }

            pom.project {
                name POM_NAME
                packaging POM_PACKAGING
                description POM_DESCRIPTION
                url POM_URL

                scm {
                    url POM_SCM_URL
                    connection POM_SCM_CONNECTION
                    developerConnection POM_SCM_DEV_CONNECTION
                }

                licenses {
                    license {
                        name POM_LICENCE_NAME
                        url POM_LICENCE_URL
                        distribution POM_LICENCE_DIST
                    }
                }

                developers {
                    developer {
                        id POM_DEVELOPER_ID
                        name POM_DEVELOPER_NAME
                    }
                }
            }
        }
    }
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from 'build/docs/javadoc'
}

task sourcesJar(type: Jar) {
    classifier = 'sources'
    from sourceSets.main.allSource
}

javadoc{
    failOnError = false
}

artifacts {
    archives jar
    archives javadocJar
    archives sourcesJar
}

signing {
    sign configurations.archives
}  
```

**NOTICE:**  
About javadoc, add

```
javadoc{
    failOnError = false
}
```

to fix javadoc error.

###Create project root gradle.properties 
Copy following, and input your information:

```
VERSION_NAME = <your version name>
GROUP = <your group>

POM_DESCRIPTION = <your description>
POM_URL = <likes https://github.com/saintdan/java_util>
POM_SCM_URL = <likes scm:github.com/saintdan/java_util.git>
POM_SCM_CONNECTION = <likes scm:github.com/saintdan/java_util.git>
POM_SCM_DEV_CONNECTION = <likes github.com/saintdan/java_util.git>
POM_LICENCE_NAME = The Apache Software License, Version 2.0
POM_LICENCE_URL = http://www.apache.org/licenses/LICENSE-2.0.txt
POM_LICENCE_DIST = repo
POM_DEVELOPER_ID = <your id>
POM_DEVELOPER_NAME = <your name>
POM_NAME = <your project name>
POM_ARTIFACT_ID = <your artifact id>
POM_PACKAGING = jar

SNAPSHOT_REPOSITORY_URL=https://oss.sonatype.org/content/repositories/snapshots
RELEASE_REPOSITORY_URL=https://oss.sonatype.org/service/local/staging/deploy/maven2
```

**NOTICE:**  
VERSION_NAME with -SNAPSHOT suffix means what published is snapshot.  

###Modify project root build.gradle
Add **apply from: 'maven-push.gradle'** at the end.

###Update your home gradle.properties.
This will include the username and password to upload to the Maven server and so that they are kept local on your machine. The location defaults to **USER_HOME/.gradle/gradle.properties**
 
It may also include your signing key id, password, and secret key ring file (for signed uploads). Signing is only necessary if you're putting release builds of your project on maven central.

```
NEXUS_USERNAME = <your username>
NEXUS_PASSWORD = <your password>

signing.keyId = <your gpg keyId>
signing.password = <your gpg purchase>
signing.secretKeyRingFile = ~/.gnupg/secring.gpg
```

##Pulish your jar to Sonatype

cd to your project root and:

```
gradle uploadArchives
```

Wait for the success notification.  
Your library will be built, signed and pushed to the sonatype repo. Please make sure that **signing** stage was not skipped. It is skipped if your library name is ending with **"-SNAPSHOT"**, but for releases signing is mandatory.

##Offcial release
Open [Sonatype Nexus Professional](https://oss.sonatype.org) and log in, click the Staging Repositories button to open the list and search your projet.  
Select it, and press **Close** button. Closing a library actually means that you're ready to release it. Another option is **Drop** button, which means removing it from the list.  
If closing went fine, you should see a **Release** button active. Press it and then get back to JIRA and post a comment there that you promoted you library.  
After that you should get a response from Sonatype that your library will be available in ~10 minutes and it will be synced with the Maven Central in the next few hours.  
**NOTICE:**  
Remeber to comment on the ticket which holds this jar when you promoted your first release.  


**Thanks for reading and happy coding.**