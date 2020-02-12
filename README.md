## android-setup-reference

**I. Renaming app package from `java` to `kotlin`**
1. In the Project window, switch view from `Android` to `Project`
2. Under `app/src/main`, right click **java**
3. Select **Refactor**, then click **Rename**
4. In the Rename dialog window's input field, enter "kotlin", then click **REFACTOR**
5. Update application `build.gradle` 
```groovy
// build.gradle (app)
android {
    sourceSets {
        main.java.srcDirs += 'src/main/kotlin/'
    }
}
```
6. Sync project with the changes

**II. Tidying up dependencies**
1. In the Project view, under the `gradle` folder, create a new file named `dependencies.gradle`
2. Open `dependencies.gradle`, add the following:

```groovy
// dependencies.gradle
ext {
    androidx_version = "1.1.0"
    constraint_layout_version = "1.1.3"
    junit_version = "4.12"
    junit_ext_version = "1.1.1"
    espresso_version = "3.2.0"

    commonDependencies = [
            "kotlinJdk7"      : "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version",
            "appcompat"       : "androidx.appcompat:appcompat:$androidx_version",
            "coreKtx"         : "androidx.core:core-ktx:$androidx_version",
            "constraintLayout": "androidx.constraintlayout:constraintlayout:$constraint_layout_version",
            "junit"           : "junit:junit:$junit_version",
            "junitExt"        : "androidx.test.ext:junit:$junit_ext_version",
            "espresso"        : "androidx.test.espresso:espresso-core:$espresso_version"
    ]
}
```

3. In your project `build.gradle`, load the file

```groovy
// build.gradle (project)
apply from: 'gradle/dependencies.gradle'
```

4. In your application `build.gradle`, add `globalConf`:
```groovy
// build.gradle (app)
apply plugin: 'com.android.application'
...

// Add this
def globalConf = rootProject.ext
```

5. Still in your `build.gradle`, utilize the dependencies:

```groovy
// build.gradle (app)
dependencies {
    Map<String, String> dependencies = globalConf.commonDependencies

    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation dependencies.kotlinJdk7
    implementation dependencies.appcompat
    implementation dependencies.coreKtx
    implementation dependencies.constraintLayout
    testImplementation dependencies.junit
    androidTestImplementation dependencies.junitExt
    androidTestImplementation dependencies.espresso
}
```
