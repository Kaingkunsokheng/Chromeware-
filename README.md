
```cmd
@echo off
:: ====================================================================
:: [PROJECT NAME: T] - DAILY CLEANER & AUTOMATIC INSTALLER
:: រត់សម្អាតស្វ័យប្រវត្តិ ១ ថ្ងៃម្តង | ប្រើប្រាស់ CPU Background 0%
:: ====================================================================

:: ប្រសិនបើប្រព័ន្ធ Windows ហៅកូដនេះមករត់ (មាន Argument -bg) ឱ្យទៅវគ្គសម្អាតភ្លាម
if "%~1"=="-bg" goto :clean

:: --------------------------------------------------------------------
:: វគ្គដំឡើង TASK SCHEDULER ស្វ័យប្រវត្តិ (រត់តែម្តងគត់ពេលអ្នកចុចដំឡើង)
:: --------------------------------------------------------------------
echo [ + ] កំពុងដំឡើងកាលវិភាគសម្អាតរៀងរាល់ ១ ថ្ងៃម្តង...

:: បង្កើត Task ឈ្មោះ "DailyUserCleaner" ឱ្យរត់រៀងរាល់ ១ ថ្ងៃម្តង (/sc daily /mo 1)
schtasks /create /tn "DailyUserCleaner" /tr "\"%~f0\" -bg" /sc daily /mo 1 /f >nul 2>&1

if %errorlevel% equ 0 (
    echo [ OK ] បានដំឡើងជោគជ័យ! កូដនេះនឹងរត់សម្អាតស្វ័យប្រវត្តរាល់ ១ ថ្ងៃម្តង។
) else (
    echo [ ERR ] មានបញ្ហាក្នុងការដំឡើង។ សូមប្រាកដថាប្រព័ន្ធសុវត្ថិភាពមិនបានបិទ schtasks។
)
pause
exit

:clean
:: --------------------------------------------------------------------
:: វគ្គសម្អាតទិន្នន័យ (រត់លាក់ខ្លួនស្ងាត់ៗ ដំណើរការដោយប្រព័ន្ធ Windows)
:: --------------------------------------------------------------------
wmic process where name="cmd.exe" call setpriority "idle" >nul 2>&1

:: សម្អាត Temp និង Internet Cache របស់ Windows
del /q /f /s %temp%\*.* >nul 2>&1
del /q /f /s "%USERPROFILE%\AppData\Local\Microsoft\Windows\INetCache\*.*" >nul 2>&1
del /q /f /s "%USERPROFILE%\AppData\Local\CrashDumps\*.*" >nul 2>&1

:: សម្អាត Cache របស់ Microsoft Edge
del /q /f /s "%LocalAppData%\Microsoft\Edge\User Data\Default\Cache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Microsoft\Edge\User Data\Default\Code Cache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Microsoft\Edge\User Data\Default\GPUCache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Microsoft\Edge\User Data\Default\Service Worker\CacheStorage\*.*" >nul 2>&1

:: សម្អាត Cache របស់ Google Chrome
del /q /f /s "%LocalAppData%\Google\Chrome\User Data\Default\Cache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Google\Chrome\User Data\Default\Code Cache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Google\Chrome\User Data\Default\GPUCache\*.*" >nul 2>&1
del /q /f /s "%LocalAppData%\Google\Chrome\User Data\Default\Service Worker\CacheStorage\*.*" >nul 2>&1

exit
```

## ហេតុអ្វីបានជាវិធីនេះល្អបំផុតសម្រាប់អ្នក?
*   **មិនស៊ី CPU ចោល:** មិនមាន Loop រត់វិលជុំពេញមួយថ្ងៃនាំឱ្យក្ដៅម៉ាស៊ីន ឬអស់ថ្មឡើយ។
*   **រត់តែម្តងគត់ក្នុងមួយថ្ងៃ:** Windows នឹងដាស់កូដនេះឱ្យរត់សម្អាតសំរាមក្នុងម៉ាស៊ីនតែម្តងគត់ក្នុងមួយថ្ងៃ រួចវានឹងបិទខ្លួនឯង (Exit) ភ្លាមៗ។
*   **មិនត្រូវការ Admin:** ដំណើរការដោយសិទ្ធិ User ធម្មតា មានសុវត្ថិភាពខ្ពស់។

<FollowUp>
តើអ្នកចង់កំណត់ **ម៉ោងជាក់លាក់** សម្រាប់ឱ្យវារត់រាល់ថ្ងៃ (ឧទាហរណ៍៖ ម៉ោង ១២ ថ្ងៃត្រង់) ឬចង់ឱ្យខ្ញុំបង្ហាញ **កូដសម្រាប់លុប (Uninstall)** កាលវិភាគនេះចេញពីម៉ាស៊ីនវិញនៅថ្ងៃក្រោយ?
</FollowUp>
