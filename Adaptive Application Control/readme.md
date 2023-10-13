# Adaptive application control

## Administer AppLocker on the local PC

1. Click Start, type local security policy, and then click Local Security Policy.

2. If the User Account Control dialog box appears, confirm that the action it displays is what you want, and then click Yes.

3. In the console tree of the snap-in, double-click Application Control Policies, double-click AppLocker, and then click the rule collection that you want to create the rule for.

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/dfba5765-3528-4233-b917-afd9ec9ce784)

Check if App Locker rules have been configured on the machine <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6b7130cf-a2fe-4b30-bccb-5a7539e60789)

A. If it is configured, check if events are logged in Event Viewer:
```
Applications and Services Logs\Microsoft\Windows\AppLocker\EXE and DLL
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/dea00900-0f8b-4d54-b459-658d62e3e450)

The pushlisher the software being launched here (take WPS as example) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/14240649-3c13-4d1b-a2a0-4b832185d367)

B. If it's not configured, check Event Viewer to see what happened with the onboarding configuration script.

Check for events of type: `9601 (info)`, `9602 (warning)`, `9603 (error)`

4. If no events exist even though AppLocker is configured - check that Application Identity service is turned on
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/1241a942-0dce-4b69-9405-0a66eb4b02d9)

5. Finally, To get effective AppLocker configuration for investigation:
```powershell
Get-AppLockerPolicy -Effective -Xml | Out-File C:\temp\AppLockerPolicy.xml
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/eb93095e-e974-4381-b1dc-8605631d30fc)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/0650c3d0-a95d-42c1-b9e2-487fe8d63e94)

