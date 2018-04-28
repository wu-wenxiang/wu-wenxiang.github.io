---
layout:         post
title:          Hololens-入门资料
category:       blog
description:    Say "Hello World" to Hololens, 整理了一下Hololens的入门资料
---

## Overview
- [The official docs](https://docs.microsoft.com/en-us/hololens/)
- [Developer readiness status](https://developer.microsoft.com/en-us/windows/mixed-reality/developer_readiness_status)
 
## Dev Environment Setup
- [Overview](https://developer.microsoft.com/en-us/windows/mixed-reality/install_the_tools)
- Unity 5.5+
- VS2015 Update 3 or VS2017
- [HoloLens Emulator](https://developer.microsoft.com/en-us/windows/mixed-reality/using_the_hololens_emulator)
- Vuforia

## Holographic Academy
- [Overview](https://developer.microsoft.com/en-us/windows/mixed-reality/academy)
	- Holograms 100: [Getting started with Unity](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_100)
		- Setup the Camera: Position, Clear Flags, Clipping Plane
		- Project Settings: Quality and Player
		- Build Settings
	- Holograms 101E: [Introduction with Emulator](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_101e)
		- Gaze
		- Gestures
		- Voice input
		- Spatial sound
		- Spatial mapping
	- Holograms 101: [Introduction with Device](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_101)
	- Holograms 210: [Gaze](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_210)
		- Cursor
		- Gaze Stabilizer
		- Directional indicator
		- Billboard
		- Tag-Along
	- Holograms 210: [Gesture](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_211)
		- Hand detected feedback
		- Navigation
		- Hand Guidance
		- Manipulation
		- Model expansion
	- Holograms 212: [Voice](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_212)
		- Awareness
			- Dos
				- Create concise commands
				- Use a simple vocabulary
				- Be consistent
			- Dont's
				- Use single syllable commands
				- Use system commands
				- Use similar sounds
		- Acknowledgement 
		- Understanding and the Dictation Recognizer
		- Grammar Recognizer
	- Holograms 220: [Spatial sound](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_220)
		- Spatial Sound and Interaction
		- Spatial Sound and Spatial Mapping
		- Sound Design
			- Sound and Experience Design 
				- Normalize all sounds
				- Design for an untethered experience
				- Emit sound from logical locations on your holograms
				- Familiar sounds are most localizable
				- Be cognizant of user expectations
				- Avoid hidden emitters
			- Sound Mixing 
				- Target your mix for 70% volume on the HoloLens
				- HoloLens at 100% volume should drown out external sounds
				- Use the Unity AudioMixer to adjust categories of sounds
				- Mix sounds based on the user's gaze
				- Building your mix
			- Performance 
				- CPU usage
				- Stream long audio files
			- Special Effects
	- Holograms 230: [Spatial mapping](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_230)
			- Scanning: SurfaceObserver
			- Visualization: Shader
			- Processing
			- Placement
			- Occlusion: Rim and Grid shader
	- Holograms 240: [Sharing holograms](
https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_240)
		- Holo World
		- Interaction
		- Shared Coordinates
		- Discovery
		- Placement
		- Real-World Physics
		- Grand Finale

## Others
- [Holographic Remoting Player](https://developer.microsoft.com/en-us/windows/mixed-reality/holographic_remoting_player)
- [HoloToolkit-Unity](https://github.com/Microsoft/HoloToolkit-Unity) -> [Getting Started](https://github.com/Microsoft/HoloToolkit-Unity/blob/master/GettingStarted.md)
- [Samples](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples) -> [With "Holographic" prefix](https://github.com/Microsoft/HolographicAcademy)
