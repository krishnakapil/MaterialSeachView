# MaterialSeachView
Android Material Design Style Custom Search View

#Usage
Copy the material-search.aar file into your project libs folder.
Add the following dependency into gradle.

repositories {
        flatDir {
            dirs 'libs'
        }
}


dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile(name:'material-search', ext:'aar')
}
