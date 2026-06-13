# Public_JavaUpgrade

## Reqwrite vs. MVN

<b>versions-maven-plugin:</b> Patch-/Minor-Bumps, „alles mal aktuell" haltenechte<br>
<b>Reqwrite:</b> Versionssprünge (Java 17→21, Boot 2→3)

### ReadOnly: Veraltete Versionen auflisten (der "IstStand"):

```js
mvn versions:display-property-updates       # Versionen über <properties>

````

```sql
[INFO] The following version property updates are available:
[INFO]   ${spring.version} .................................... 7.0.0 -> 7.0.8
[INFO]   ${tomcat.version} ................................ 11.0.1 -> 11.0.22
````
<br><br>

```js
mvn versions:display-dependency-updates    # fest verdrahtete <version>
````

```sql
[INFO] The following version property updates are available:
[INFO]   org.springframework:spring-aop ........................ 7.0.0 -> 7.0.8
[INFO]   org.springframework:spring-aspects .................... 7.0.0 -> 7.0.8
[INFO]   org.apache.tomcat.embed:tomcat-embed-core ......... 11.0.1 -> 11.0.22
````

```js
mvn versions:display-plugin-updates         # Plugin-Versionen
````
....

### Writes Code

Rewrite: 
```js
mvn versions:use-latest-releases            # hebt Dependencies an
mvn versions:update-properties              # hebt Versions-Properties an
mvn versions:use-latest-versions            # inkl. Snapshots/Milestones (Vorsicht)
````

```js
mvn versions:use-latest-releases            # hebt Dependencies an
mvn versions:update-properties              # hebt Versions-Properties an
mvn versions:use-latest-versions            # inkl. Snapshots/Milestones (Vorsicht)
````
