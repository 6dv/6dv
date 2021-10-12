# 6dv

A file format built for live streaming 3D generated content to standalone VR headsets in 6DOF.

<a href="https://discord.gg/RHprFfuBte"><img alt="discord" src="https://img.shields.io/badge/Discord-Community-green"/></a>

------

.6dv stands for **6** **D**egree of freedom **V**ideo. 

.6dv was built from the ground up with a focus on streaming dynamic 3D generated content to standalone VR headsets that are limited in processing power.

Checkout a demo here: https://parched-seat.surge.sh/?s=097aef

Example `6dv` included in the project

## Benefits
*  Live streaming/vod playback supported
*  Bandwidth less than 1/10 of an equivalent volumetric video
*  Interactions can be baked into the file itself letting viewers perform lightweight actions to engage with the content(see the demo above for an example)


## General Structure of a .6dv
File is built out of this general structure, e.g. each one of these is bytes from start -> finish in the file:

* File Signature (2 bytes) [uint16]: so that we now we're working with 6dv (see file signatures) 0xBEEF
* Meta Size (4 bytes) [uint32]: The size of the metadata payload
* Meta ByteString (meta_size bytes): A msgpack metadata object containing the info specified in Header Format
* A series of repeating Frame Sequences. Each frame constitutes a contiguous block of bytes which can be read in as individual frames

## Header Format

| Property  | Num Bytes | Type | Notes |
| ------------- | ------------- | -------------| -------------|
| numFrames  | 4  | uint32 | All the frames in the file |
| fps  | 2  | uint16 | |




## Frame Sequence format
Each frame sequence has to have the following structure:

1. Frame start marker (1 byte) [uint8]: A unique random byte to indicate the start of a frame `0xF0`
1. Frame type (1 byte) [uint8]: 0 for IFrame and 1 for PFrame
1. Frame size (4 bytes) [uint32]: The size of all the frame bytes
1. Frame data (frame_size bytes): The actual frame data. This could either be:
    1. An IFrame, raw bytes from an glTF GLB file
    1. A PFrame, msgpack JSON diff object represented using the same encoding as the metadata header.

## Example
![frame_marker_diagram](https://user-images.githubusercontent.com/8617779/135314518-603c867f-2aeb-48bb-8f72-398a845c17b7.jpg)

## IFrames and PFrames

6DV is heavily influenced from the 2D live video streaming tech called DASH: https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP

IFrames describe a full snapshot. The snapshot is a `.glb` file that fully describes the scene.

Pframes are a bJSON tree that describe the new state of each property that has changed since the last frame

By loading an I Frame, then applying a series of P Frames to the existing state, we can create a 3D video that fully describes a scene.


## Contributing

Any way you want to contribute is a good way!

The best place to start is to join the discord and Dan(dan#9955) will help you get setup. I'm working on documentation to help make this easier!

Even if you don't plan to submit any code, just joining the discussion on Discord and giving your feedback helps a lot. New Ideas are always welcome!

You can also ⭐ Star this repo to show your interest/support.

## Features & Roadmap

Supported Features 
* Realtime Lights (Spot, Directional, Point)
* Meshes
* Skinned Meshes
* Unchanging Textures
* Emissive Materials

Roadmap
* Lightmaps
* Particles
* Skyboxes
* Render Textures
* Custom Shaders


## Upcoming open source repos

Over the next few months i'll be open sourcing several tools I've built over the last year that make the generation and playback of these files a lot easier.

* Unity -> .6DV SDK
* Unity -> primitive extracter
* .6dv Web Player built in WebXR
* Aframe .6dv player
* Desktop tools for editing .6dv files to remove/add effects
