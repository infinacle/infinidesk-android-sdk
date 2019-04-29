# Infinidesk Android Sdk

## Getting started
A new Android project with empty activity theme will be created to display the steps of SDK integration.
1. Create a new project with empty activity theme in Android studio.

2. Open project’s ```build.gradle``` file and add the following code in ```allprojects -> repositories``` section.
```java
maven { 
  url 'https://jfrog.infinacle.com/artifactory/libs-release-local'
}
```

3. Open ```app/build.gradle``` file, add the following code. 
```java
android{
  ...
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  } 
}

dependencies { 
  implementation 'com.infinihelpdesk:InfiniHelpDesk:1.0.1'
}
```

4. With Android Studio 3.2 and higher, you can quickly migrate an existing project to use AndroidX by selecting **Refactor > Migrate to AndroidX** from menu bar.

5. Navigates to ```gradle.properties``` file, add the following code.

```java
android.useAndroidX = true
android.enableJetifier = true
```
6. Clean and rebuild the project after adding these.

7. Creates a button and set an idReference for it in the ```activity_main.xml``` layout file. Next, link the button in the **onCreate()** method in ```MainActivity.java``` file. Sample code as below.
```java
openButton = findViewById(R.id.button);
openButton.setOnClickListener(v -> 
  openInfiniChat(idReference, idReferenceType, apiKey, name, email, language, uat)
);
```

8. Open up the ```MainActivity.java``` file and follow the guide below:<br />
This app will need to pass in 7 string values to generate a token in order to proceed to websocket request.
- ```idReference``` You may pass in visitor email or a GUID. The value that sets here must be unique.<br />
- ```idReferenceType``` A reference type, eg. guest<br />
- ```apiKey``` Pass in a unique api key.<br />
- ```name``` Pass in visitor name<br />
- ```email``` Pass in visitor email<br />
- ```language``` Pass in value "en-US"<br />
- ```uat``` Pass in value "true"<br /><br />
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
  Below is the function to run and open the Infinicle chat application:
```java
openInfiniChat(String idReference, String idReferenceType,
                  String apiKey, String name,
                  String email, String language,
                  String uat){
  InfiniHelpDeskIntent infiniDesk = new InfiniHelpDeskIntent();
  infiniDesk.setIdRef(idReference);
  infiniDesk.setIdRefType(idReferenceType);
  infiniDesk.setApiKey(apiKey);
  infiniDesk.setName(name);
  infiniDesk.setEmail(email);
  infiniDesk.setLang(language);
  infiniDesk.setUat(uat);

  i = InfiniHelpDesk.getInstance().startChat(infiniDesk, this);
  startActivityForResult(i, 1);
}
```

9. You should be able to run your app now. Else, invalidate caches and restart Android Studio.
