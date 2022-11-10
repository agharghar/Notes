```shell
JuicyPotato.exe -t t -p C:\Users\Public\whoami.exe -l 5837
```

*The first required flag ( -t ) is the "Process creation mode". The documentation states that we need _CreateProcessWithToken_ if we have the _SeImpersonate_ privilege, which we do. To direct Juicy Potato to use _CreateProcessWithToken_, we will pass the t value.

*Next, the -p flag specifies the program we are trying to run. In this case, we can use the same backdoored whoami.exe binary that we used previously.

*Finally, Juicy Potato allows us to specify an arbitrary port for the COM server to listen on with the -l flag.
