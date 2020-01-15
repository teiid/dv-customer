# Example VDB as Maven Artifact (dv-customer)
Example project to show to create a VDB as a maven artifact from a single or multiple DDL file(s)

In this model, VDB can be defined in exploded format, as in, a single DDL file does not need to contain all the schema for every model, a separate DDL files can be defined. You can also include additional library files in "lib" directory. However, there *MUST* be one main vdb file called `vdb.ddl` in the `src/main/vdb` folder. This vdb folder can contain any number of sub-folders as needed to separate the code as needed. When the maven build is runs, it will scan through `src/main/vdb` folder and builds an artifact with name `${project.name}-${version}.vdb` file, and also places this artifact in the maven repository.

When build runs, it will also resolve any `vdb-import` statements in the main VDB, and pull in the contents for the imported vdbs from additional vdb files that are defined in the `src/main/vdb` folder like for example `/src/main/vdb/import/foo-vdb.ddl` where the `foo` vdb is imported in your main VDB. Alternatively, you can define a maven dependency like


```
<dependency>
    <groupId>com.example</groupId>
    <artifactId>customer-vdb</artifactId>
    <version>${project.version}</version>
    <type>vdb</type>
</dependency>
```

in the `pom.xml` file as dependency, and the plugin will find the file and resolve the contents appropriately.

NOTE: `vdb-import` are only allowed at top level vdb, no nesting allowed. If nesting is needed, build the intermediate VDBs as top level vdb maven artifacts, and then use them as imports into others, essentially keeping the nesting to single level.

All above is to build a VDB that can be used else where with Operator for deployment, or to be imported into other VDBs.

To Build, simply run

```
mvn clean install
```

that should generate a `${project.name}-${version}.vdb` file in your target repository. If you want deploy this artifact to a remote repository then you need to call 

```
mvn clean install deploy
```

it is also expected that the user provides the respository locations and necessary user credentials in the pom.xml file before `deploy` is run.
