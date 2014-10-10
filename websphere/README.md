## Notes on using WSA Admin for WebSphere 7

### Start WSA Admin (pick jacl or jython)

      /opt/IBM/WebSphere/Profiles/DefaultDmgr01/bin/wsadmin.sh -lang jacl


### Starting/stopping apps

      set appManager [$AdminControl queryNames cell=CELL,node=NODE,type=ApplicationManager,process=SERVER1,*]
      $AdminControl invoke $appManager stopApplication APP1
      $AdminControl invoke $appManager startApplication APP1 


### Generating dumps

      set jvm [$AdminControl completeObjectName type=JVM,process=SERVER1,*]
      $AdminControl invoke $jvm generateHeapDump
      $AdminControl invoke $jvm dumpThreads 

