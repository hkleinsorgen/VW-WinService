# VW-WinService
Allows to run an image as a Windows service. 

## Usage
Upon startup, the image needs to start the service handler:
```
(WinService serviceName: 'MyService')
    startAsService: [ " start the service, e.g. open sockets " ]
    postStartBlock: [ " evaluated after the socket has started " ]
    shutdownBlock: [ " shut down, e.g. close sockets " ].
```
This method does not return. 
The block startAsService should be as brief as possible. After the block has been evaluated, WinService sets the service status to `running`. 
When startup fails, a WinServiceStartupError will be raised. The default action of this signal is to quit the image.
Additional initializations can be performed in the postStartBlock.
The shutdown block will be evaluated when the service is stopped (either by stopping the service or shutting down Windows). After the block has been evaluated, WinService sets the service status to `stopped`. 
Startup and shutdown should not exceed a runtime of 15 seconds, otherwise Windows will report an error.

To install a service:
```
(WinService serviceName: 'MyService')
    installServiceWithParameters: ''   " string with command line parameters send to the executable " 
    prerequisistes: #()   " collection of service names " 
    autoStart: true " true if the service should be automatically started by Windows on startup " 
    startNow: false " true if the service should be started after installation " 
```
To deinstall a service:
```
(WinService serviceName: 'MyService')
    deinstallService
```
which is equivalent to entering `sc delete MyService` in a shell.

The current directory of the image is `%SYSTEMROOT%\system32` or `%SYSTEMROOT%\SysWOW64`,
so you might want to change the current directory on startup, e.g.
```
CEnvironment commandLine first asFilename directory beCurrentDirectory.
```

## Contact

c . schuckmann -at- i-views . de

h . kleinsorgen -at- i-views . de
