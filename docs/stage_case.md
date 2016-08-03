# Stage Publish Case Study

SMV has this [Stage-Publish](https://github.com/TresAmigosSD/SMV/blob/master/docs/user/smv_stages.md)
mechanism which may not be used for many small projects or projects worked by small teams. However it is
very useful function of SMV for large projects with multiple team coordination.

Here is a real project example where we started as a small team and with it grew we stated running into
coordination issues, and have to move to a solution by utilizing the Stage-Publish mechanism.

## Project Background

The project was started as a specific client use case. We need to use client internal data and some public
domain data to create some data model to support a couple of client business use cases as modeling effort.

The project started as a single piece, but because of the data actually coming from different sources, it
had been split to 2 small teams from 2 different locations.
I will call the team who handles the public domain data TeamA, and the
team who handles the client internal data as TeamB.

Since TeamA only handle public domain data, they don't need to have access to client server. Instead they
mainly work on our own data server, and the public data are hosted on that server. And potentially the
public domain data can support not only TeamB but other client project teams.

For TeamB, since their final output data modules depends on both client internal data and TeamA's output,
before we adopt the Stage-Publish mechanism, TeamB's project code has Library-level dependency
to TeamA's code. In other words, when TeamB compile their code, they actually compiled in TeamA's code to
the JAR file. Because of that, when TeamB run their code on the data, they also need a copy of the raw
input files of the public domain data.

## The Problem We Ran into

Since TeamA's responsibility is decoupled from direct client deliverable, they were required to keep the
public domain data project moving forward. On one side, TeamA have to make changes to adapt to data
source changes, such as new data coming in, some old data need to be fixed based on new knowledge, etc.;
on the other side, TeamB want to have a stable input from public domain side.

The problem getting worse when the 2 copies of the raw public domain data get out of sync. TeamB members
frequently running into "file not found" error at run time.

## The Solution

The `Stage-Publish` mechanism was designed for this type of issues.

For solving the problem we took the following steps:

* TeamA checks project code's (TeamA and TeamB could work on different Scala projects or the same
  Scala project) `smv.stages` in `conf/smv-app-conf.props` file, to make sure that all the stages
  TeamB depends on are listed in that configuration parameter
* TeamA checks `smv.publishDir` setting in `smv-user-conf.props` either under project `conf` dir or
  `~/.smv` to confirm it is the desire location we want to store the published data
* TeamA run `smv` with stage publish and provide a "version" string for that stage's published version
* TeamA announce the "release" of the published data and published location
* TeamB sync the published data to their server's `smv.publishDir`
* TeamB specify `smv.stages.{stage name}.version` to be the published version string in their
  `smv-user-conf` for all the stages TeamB depends on

Here is an example of an existing TeamB's `smv-user-conf`:
```
smv.stages.geo.version = TCD.20160802.geo
smv.stages.cms.version = TCD.20160802.cms
smv.stages.trial.version = TCD.20160802.trial
```

Basically, TeamB's code depends on 3 stages from TeamA: "geo", "cms", and "trail". Please note that
here we just use the last part of the Fully Qualified Name of the stage packages. Later version of
SMV will support to use the FQN itself to remove any potential ambiguity.
The so-called version (TCD.20160802.geo, etc.) here, from TeamB point of view is the directory name
under `smv.publishDir` which synced from TeamA's published dir.

As a long term best practice, TeamA should run stage publish periodically, and announce each time
through some standard team coordination channel, email, Slack, Confluence, etc. And TeamB should to
determine whether to take the new released data or not.  
