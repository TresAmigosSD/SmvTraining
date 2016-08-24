# Simple AdHoc Analysis within Smv Shell

## Some useful commands in smv_shell
Smv-shell has a `help` function, which simply list all the shell functions/commands.  
```
scala> help
Here is a list of SMV-shell command

Please refer to the API doc for details:
http://tresamigossd.github.io/SMV/scaladocs/index.html#org.tresamigos.smv.shell.package

* lsStage
* ls
* ls(stageName: String)
* lsDead
* lsDead(stageName: String)
* lsLeaf
* lsLeaf(stageName: String)
* graph
* graph(ds: SmvDataSet)
* graph(stageName: String)
* ancestors(ds: SmvDataSet)
* descendants(ds: SmvDataSet)
* peek(df: DataFrame, pos: Int = 1)
* now
```

## Resolve an SmvDataSet (SmvModule or SmvFile)
If there were no changes on the project code after smv-shell started, one can use the
standard way to load an `SmvDataSet`
```scala
scala> val a=df(stage1.EmploymentByState)
a: org.apache.spark.sql.DataFrame = [ST: string, EMP: bigint]
```

If changes are made on the code of the module, and re-compiled, please use the dynamic
loading method,
```scala
scala> val a=ddf("com.mycompany.MyApp.stage1.EmploymentByState")
a: org.apache.spark.sql.DataFrame = [ST: string, EMP: bigint]
```
About dynamic loading,
please refer https://github.com/TresAmigosSD/SMV/blob/master/docs/user/class_loader.md for details.

## Some useful DataFrame methods and helpers

### Show what are in a DataFrame
```
scala> val myDf=df(stage1.EmploymentByState)
myDf: org.apache.spark.sql.DataFrame = [ST: string, EMP: bigint]

scala> myDf.printSchema
root
 |-- ST: string (nullable = true)
 |-- EMP: long (nullable = true)


scala> myDf.peek
ST:String = 50
EMP:Long  = 245058
```

Without parameter, the `peek` command shows the first row of the DF, or one can
specify the row number to show.

### Show basic statistics
```scala
scala> myDf.edd.summary().eddShow
ST                   Non-Null Count         52
ST                   Min Length             2
ST                   Max Length             2
ST                   Approx Distinct Count  52
EMP                  Non-Null Count         52
EMP                  Average                2170425.7884615385
EMP                  Standard Deviation     2330941.3442028034
EMP                  Min                    202724.0
EMP                  Max                    1.2319102E7
```

### Basic histograms

Histogram on a Sting field
```scala
scala> myDf.edd.histogram("ST").eddShow
Histogram of ST: String sort by Key
key                      count      Pct    cumCount   cumPct
01                           1    1.92%           1    1.92%
02                           1    1.92%           2    3.85%
04                           1    1.92%           3    5.77%
05                           1    1.92%           4    7.69%
06                           1    1.92%           5    9.62%
08                           1    1.92%           6   11.54%
09                           1    1.92%           7   13.46%
......
```

Histogram on a number field with binning specified
```scala
scala> myDf.edd.histogram(edd.Hist("EMP", binSize=1000000)).eddShow
Histogram of EMP: with BIN size 1000000.0
key                      count      Pct    cumCount   cumPct
0.0                         20   38.46%          20   38.46%
1000000.0                   10   19.23%          30   57.69%
2000000.0                   11   21.15%          41   78.85%
3000000.0                    4    7.69%          45   86.54%
4000000.0                    3    5.77%          48   92.31%
6000000.0                    1    1.92%          49   94.23%
7000000.0                    1    1.92%          50   96.15%
8000000.0                    1    1.92%          51   98.08%
1.2E7                        1    1.92%          52  100.00%
-------------------------------------------------
```

### Export EDD result to file
```scala
scala> myDf.edd.histogram(edd.Hist("EMP", binSize=1000000)).eddSave("EMP.hist")
```
