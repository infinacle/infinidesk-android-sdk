
<p align="left">
  <a href="https://desk.infinacle.com/">
    <img alt="infinidesk" src="https://infinacle.com/wp-content/uploads/2018/10/footer_logo_100x100.png" width="150">
  </a>
</p>

Streamline your customer experience with scalable real-time chat

# Infinidesk Android Sdk
#### Getting started

## Table of Contents
1. [Add the InfinacleChat Dependency ](#Add-Dependency)
2. [Migrate Current to AndroidX ](#Migrate-Current)
3. [Start the Sdk ](#Start-Sdk)

## Add the InfinacleChat Dependency

1. Open **project level** ```build.gradle``` file, add the following code in the ```repositories``` block under the ```allprojects``` section.
```java
allprojects {
  repositories {
    ...
    maven { 
      url 'https://jfrog.infinacle.com/artifactory/libs-release-local'
    }
  }
}
```

2. Open **app level** ```build.gradle``` file, add the following code in the ```android``` section. 
```java
android{
  ...
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  } 
}
```

3. Open **app level** ```build.gradle``` file, add the following code in the ```dependencies``` section. 
```java
dependencies { 
  ...
  implementation 'com.infinihelpdesk:InfiniHelpDesk:1.0.1'
}
```
## Migrate Current to AndroidX

1. With Android Studio 3.2 and higher, you can quickly migrate an existing project to use AndroidX by selecting **Refactor > Migrate to AndroidX** from menu bar.

2. Navigates to ```gradle.properties``` file, add the following code.

```java
android.useAndroidX = true
android.enableJetifier = true
```
3. Clean and rebuild the project after adding these.
  
## Start the Sdk

9. Open up the ```MainActivity.java``` file and follow the guide below:<br />
This app will need to pass in 5 string values to generate a token in order to proceed to websocket request.
- ```idReference``` You may pass in visitor email or a GUID. The value that sets here must be unique.<br />
- ```idReferenceType``` You may pass in visitor type, Example. **guest**<br />
- ```apiKey``` You can obtain your API key from Infinacle Chat Dashboard.<br />
- ```name``` You may pass in visitor name.<br /> 
- ```email``` You may pass in visitor email.<br /><br /> 
    Sample:<br />
    Below is the code to generate a unique GUID. This unique code will then be set as the idReference.
```java
public synchronized static String createIdReference(Context context) {
  if (idReference == null) {
    SharedPreferences sharedPrefs = context.getSharedPreferences(PREF_UNIQUE_ID, Context.MODE_PRIVATE);
    idReference = sharedPrefs.getString(PREF_UNIQUE_ID, null);
    if (idReference == null) {
      idReference = UUID.randomUUID().toString();
      SharedPreferences.Editor editor = sharedPrefs.edit();
      editor.putString(PREF_UNIQUE_ID, idReference);
      editor.commit();
    }
  }
  return idReference;
}
```
  Sample:<br />
  Add the line of code below to set the reference ID inside onCreate() method.
```java
idReference = createIdReference(this);
```
  Below is the function to run and open the Infinacle chat application:
```java 
InfiniHelpDeskIntent infiniDesk = new InfiniHelpDeskIntent();
infiniDesk.setIdRef(idReference);
infiniDesk.setIdRefType(idReferenceType);
infiniDesk.setApiKey(apiKey);
infiniDesk.setName(name);
infiniDesk.setEmail(email);

i = InfiniHelpDesk.getInstance().startChat(infiniDesk, this);
startActivityForResult(i, 1);
```
