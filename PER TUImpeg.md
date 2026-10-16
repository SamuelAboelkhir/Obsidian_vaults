---
tags:
- Go
- Projects
MOC: Personal
---
[[_0000 Home|Home]] | [[_0003 Personal MOC]] | [[PER Project ideas index|Back to index]]
# Flowchart

```plantuml
@startuml
start
:User launches app;
repeat
	:User is asked to provide the path to a audio;
	:Vido is analyzed by ffmpeg and user is shown the result;
	:User is give a playback bar to cut the audio;
	:User selects time range and confirms;
	:User is asked to pick an output location;
	:Video is saved;
	:User is asked to input another video or quit;
repeat while (User adds another video) is (yes);
->no;
stop
@enduml
```
# Progress
- [x] Add file picker
- [ ] Add time range bar
- [ ] Add input and output file values
- [ ] Check Lipgloss