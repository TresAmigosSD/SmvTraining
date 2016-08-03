# SmvTraining
SMV training track.  Follow the training tasks below to gain familiarity with SMV and Spark.  This is not meant as an exhaustive introduction to SMV and Spark, but it is a quick way to learn the essentials.

Upon completion of this training, user will have basic understanding of SMV + Spark and should have a fully setup environment to do further development.

By forking this project and commiting changes to user project, we can help with individual questions on training progress.

# Training Tasks
1. Learn basic Git/Github operations
  * Create a github account.
  * Fork this project (do **NOT** just clone it) into your own github account.
  * Clone the forked project into local machine.
  * Create an issue for this task
  * Make some changes on this readme file on your local project dir, commit the change, and push (no need for pull request)
  * Close the issue for this task. All the following tasks need to have an "issue" created
2. Follow [Smv User Guide - Installation](https://github.com/TresAmigosSD/SMV/blob/master/docs/user/smv_install.md) to setup SMV
3. Follow [Smv User Guide - Get Start](https://github.com/TresAmigosSD/SMV/blob/master/docs/user/getting_started.md) to setup this project.
  * Project Name should be SmvTraining and FQN should be `org.tresamigos.smvtraining`
  * Move everything from the created project directory to this cloned project directory and commit and push
  * Update the SMV config param smv.dataDir in the file conf/smv-user-conf.props in the project directory to reflect the new location after the project directory has been moved. See [Smv Application Configuration](https://github.com/TresAmigosSD/SMV/blob/master/docs/user/app_config.md) for further details.
4. Make some small changes on the example modules, save it, compile (mvn package), and run with either smv-run or smv-shell. May need to learn a little about Maven at this stage
5. Pickup some basic Scala: [Scala For The Impatiant](https://www.dropbox.com/s/tdc0xxv6hc0375l/Scala%20for%20the%20Impatient%20-%20Cay%20S.%20Horstmann.epub?dl=0)
6. Go through [Spark SQL and Dataframe programing guide](http://spark.apache.org/docs/1.5.2/sql-programming-guide.html) and [Spark Scala API doc](http://spark.apache.org/docs/1.5.2/api/scala/index.html#org.apache.spark.package), majorly the methods in the [DataFrame Class](http://spark.apache.org/docs/1.5.2/api/scala/index.html#org.apache.spark.sql.DataFrame) and functions in the [functions package](http://spark.apache.org/docs/1.5.2/api/scala/index.html#org.apache.spark.sql.functions$)
7. Make some changes on the example modules by trying out some of the Spark functions
8. Go through the rest of SMV User Guide
9. Write a UDF function and apply it to a column of a Spark DataFrame
