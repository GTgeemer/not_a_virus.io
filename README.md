# not_a_virus.io
not a virus.exe
if ((A_Is64bitOS=1) && (A_PtrSize!=4))
hMod := DllCall("LoadLibrary", Str, "hyde64.dll", Ptr)
else if ((A_Is32bitOS=1) && (A_PtrSize=4))
hMod := DllCall("LoadLibrary", Str, "hyde.dll", Ptr)
Else
{
MsgBox, Mixed Versions detected!`nOS Version and AHK Version need to be the same (x86 & AHK32 or x64 & AHK64).`n`nScript will now terminate!
ExitApp
}

if (hMod)
{
hHook := DllCall("SetWindowsHookEx", Int, 5, Ptr, DllCall("GetProcAddress", Ptr, hMod, AStr, "CBProc", ptr), Ptr, hMod, Ptr, 0, Ptr)
if (!hHook)
{
MsgBox, SetWindowsHookEx failed!`nScript will now terminate!
ExitApp
}
}
else
{
MsgBox, LoadLibrary failed!`nScript will now terminate!
ExitApp
}

MsgBox, % "Process ('" . A_ScriptName . "') hidden!"
Return

=::ExitApp

RunAsAdmin()
{
Global 0
IfEqual, A_IsAdmin, 1, Return 0

Loop, %0%
params .= A_Space . %A_Index%

DllCall("shell32\ShellExecute" (A_IsUnicode ? "":"A"),uint,0,str,"RunAs",str,(A_IsCompiled ? A_ScriptFullPath : A_AhkPath),str,(A_IsCompiled ? "": """" . A_ScriptFullPath . """" . A_Space) params,str,A_WorkingDir,int,1)
ExitApp
}

ExitSub:
if (hHook)
{
DllCall("UnhookWindowsHookEx", Ptr, hHook)
MsgBox, % "Process unhooked!"
}
if (hMod)
{
DllCall("FreeLibrary", Ptr, hMod)
MsgBox, % "Library unloaded"
}
ExitApp



Script :

#NoEnv
SendMode Input
SetWorkingDir %A_ScriptDir%

; SS
F11::Suspend

; NR
~LButton::
While GetKeyState("LButton") {
MouseMove(0, 1)
Sleep, 300
MouseMove(0, 1)
Sleep, 300
}
Return

Insert::ExitApp

MouseMove(X, Y) {
Return, DllCall("mouse_event", uint, 1, int, x, int, y, uint, 0, int, 0)
}

