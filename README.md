<img width="1283" height="179" alt="grafik" src="https://github.com/user-attachments/assets/ea8e2395-943d-46e5-a811-caf82c154ac4" /># Public_JavaUpgrade

# Zusammenfassung
 Nutze Maven (mvn) für Minor updates und OpenRewrite für größere, das OR auch im Code ändern kann.<br> 
 mvn hat das Flag -DallowMajorUpdates=false damit er nicht auf den Major upgradet.<br>
 mvn wird 2 Fach aufgerufen (Properties und Dependecies)<br>
 OpenRewrite: Die Inkompatibilitäten, die dir (mvn) update-properties hinterlassen würde, arbeitet das Recipe (zum Großteil) gleich mit ab.<br>
 
### Quickrun

```js
mvn versions:use-latest-releases -DallowMajorUpdates=false # nur Minor + Patch (kein Major-Sprung 6.x -> 7.x):
mvn versions:update-properties -DallowMajorUpdates=false # nur Minor + Patch (kein Major-Sprung 6.x -> 7.x):
mvn clean verify

# Openrewrite
mvn -U org.openrewrite.maven:rewrite-maven-plugin:run -Drewrite.recipeArtifactCoordinates=org.openrewrite.recipe:rewrite-spring:RELEASE  -Drewrite.activeRecipes=org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_5
````





<hr>


## Upgradeables

- Dependency-Versionen
- Versions-Properties // ist eine Variable
- Plugin-Versionen (per hand)
### Reqwrite vs. MVN
<b>versions-maven-plugin:</b> Patch-/Minor-Bumps, „alles mal aktuell" haltenechte<br>

<b>Reqwrite:</b> Versionssprünge (Java 17→21, Boot 2→3)

### ReadOnly: Veraltete Versionen auflisten (der "IstStand"):


##### Dependency-Versionen

```js
mvn versions:display-property-updates       # Versionen über <properties>
````

```sql
[INFO] The following version property updates are available:
[INFO]   ${spring.version} .................................... 7.0.0 -> 7.0.8
[INFO]   ${tomcat.version} ................................ 11.0.1 -> 11.0.22
````

<br>

##### Properties-Versionen




```js
mvn versions:display-dependency-updates    # fest verdrahtete <version>
````

```sql
[INFO] The following version property updates are available:
[INFO]   org.springframework:spring-aop ........................ 7.0.0 -> 7.0.8
[INFO]   org.springframework:spring-aspects .................... 7.0.0 -> 7.0.8
[INFO]   org.apache.tomcat.embed:tomcat-embed-core ......... 11.0.1 -> 11.0.22
````
<br>
  
```js
mvn versions:display-plugin-updates         # Plugin-Versionen
````
....

### Write : Veraltete Versionen Heben

Rewrite: 

```js
mvn versions:use-latest-releases            # hebt Dependencies an
=> mvn versions:use-latest-releases -DallowMajorUpdates=false # nur Minor + Patch (kein Major-Sprung 6.x -> 7.x):
mvn versions:use-latest-releases -DallowMinorUpdates=false # nur Patch (kein Minor, kein Major) -- am konservativsten:

mvn versions:update-properties              # hebt Versions-Properties an
=> mvn versions:update-properties -DallowMajorUpdates=false # nur Minor + Patch (kein Major-Sprung 6.x -> 7.x):
mvn versions:use-latest-versions            # inkl. Snapshots/Milestones (Vorsicht)
````

<br>
Plungins

```js

````
