# Best Practice of Directory Structure of a SMV Project

## Dir structure should be consistent with FQNs
Regarding the dir structure and file structure of Scala is not as strict as in Java, we should still keep the dirs consistent with the FQNs of packages. For example, the code for any classes (or objects) within package `org.myteam.myproject.abc` should be put under `src/main/org/myteam/myproject/abc/`.

Instead of require one file one class, we actually allow multiple classes (objects) in a single file. It's pretty flexible to organize classes (majorly SmvModules) in files within each directory.

## Organize Dir and packages horizontally
Typically we organize the project horizontally first to layers, and then within each layer, modules are organized in verticals (different input data tables or different output tables). Most small and medium sized projects have 3 layers: etl, data model, use cases. The names are not critical, but basically the contents within each of those layers are similar:

* Etl: connect to raw input tables, clean up, and could have some joins if the results of the joins will be the only data which will be accessed down stream
* Data Model: all types of relational algebra operations across modules from the Etl layer, and the target to to provide some relatively generic data model output to support multiple use cases (could be different models or BI applications)
* Use cases: all use cases should depends on the output of Data Model layer (could also depend on Etl layer directly). Different use cases should be relatively independent from one to another

For the above layer structure, the middle layer, Data Model, typically has the most inter connections. In that sense it typically within a single package (so that within a single directory). The Etl layer in most cases are relatively simple so we call put all input data in the same etl package or create sub-packages under etl package. For the Use Case layer, we typically put each use case in a separate package.

The following is a dir (so that package) structure which follows above rules:
```
myproject
  - etl
    - account
    - trnx
  - dm
  - fraud
  - risk
  - acctui
```
We assumed there are 3 use cases in this project.
