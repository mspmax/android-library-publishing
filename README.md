# Android Library Publishing - Bintray Jcenter
This is created to showcase publishing an Android library(Kotlin/Java) in Jcenter bintray

## Steps
1. Create an Account in [bintray](https://bintray.com/)
2. Create a maven repository
    * Create a package in the new maven repo
    * Add all the required info as you please (this is pretty straightforward)
    * Goto the package and add a new version
3. In the project `build.gradle` file add the following dependencies(latest versions are added at the time of writing)
    ``` groovy
    buildscript {
        dependencies {
            ....
            classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.4'
            classpath 'com.github.dcendents:android-maven-gradle-plugin:2.1'
        }
    }
    ```
4. Add the above plugins to library module `build.gradle` file,
    ``` groovy
    apply plugin: 'com.jfrog.bintray'
    apply plugin: 'com.github.dcendents.android-maven'
    ```
5. Add the following extension block in your library module `build.gradle` file
    ``` groovy
   ext{
    // Bintray information - following data is based on https://bintray.com/mspmax/sample/com.pandu

    bintrayRepo = "sample"          // repo name
    bintrayName = "com.pandu"       // package name

    libraryName = "sample"          // android library module name

    // This section specifies the group ID, artifact ID, and version number respectively
    // eg: implementation 'group:artifact:version'
    publishedGroupId = 'com.pandu'
    artifact = 'sample'
    libraryVersion = '1.0.0'        // version created in bintray package

    // Library information
    libraryDescription = 'Sample library to test bintray publishing'
    siteUrl = 'https://github.com/mspmax/SampleLibraryPublishing'
    gitUrl = 'https://github.com/mspmax/SampleLibraryPublishing.git'
    developerId ='mspmax'
    developerName = 'panduka de silva'
    developerEmail = 'test@gmail.com'
    licenseName = 'The Apache Software License, version 2.0'
    licenseUrl = 'https://www.apache.org/licenses/LICENSE-2.0'
    allLicenses = 'Apache-2.0'
   }
    ```
6. Add these custom gradle tasks which helps in uploading library binraies to bintray,
    ``` groovy
    if(project.rootProject.file('local.properties').exists()){
        apply from: 'https://raw.githubusercontent.com/nuuneoi/JCenter/master/installv1.gradle'
        apply from: 'https://raw.githubusercontent.com/nuuneoi/JCenter/master/bintrayv1.gradle'
    }
    ```
7. Add the following info to your local.properties file
    ```
    bintray.user = bintray_user
    bintray.apikey = bintray_API_key 
    ```
    API key can be taken from navigating [here](https://bintray.com/profile/edit/organizations)
8. After completing all the above steps, try running `./gradlew build` on your android studio terminal. If successful please proceed to step 9. Else, if you come across this error ` Task :sample:javadoc FAILED` then add the following code block to your project `build.gradle` file
   ``` groovy
   subprojects {
       tasks.withType(Javadoc).all { enabled = false }
   }
   ```
   This is just a temp fix and will be resolved soon
9.  After successful build you can run `./gradlew bintrayUpload` if successful you will see the library version available in bintray. you're welcome ! :tada: :punch: :muscle:   if not, please let me know !

## More steps - adding to Jcenter
Congratulations, your library is now online and is ready for anyone to use it ! However, we are not done....just yet. The library is still on your own Maven Repository not on jcenter yet. So, if anyone want to use your library, they have to define the repository's url first like below in there project `build.gradle` file(which is bit annoying TBH),

   ``` groovy
   repositories {
       maven {
           url 'https://dl.bintray.com/your_bintray_username/maven/'
       }
   }
   ```
This is not really recommended as it'll be bit messy for anyone who uses your library. So one step left, goto your bintray package web interface and clock on add to Jcenter. Thats it !

**NOTE** : Only open source(public) repos are free to publish in Jcenter, for private repos you have to get a paid account from bintray. However, for private repos you can still use it via your bintray maven repo.

License
-------

    Copyright 2019 Panduka De Silva

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
