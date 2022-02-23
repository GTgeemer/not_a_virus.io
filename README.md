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
