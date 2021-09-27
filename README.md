# 6dv

## General Structure
File is built out of this general structure, e.g. each one of these is bytes from start -> finish in the file:

File Signature (2 bytes) [uint16]: so that we now we're working with 6dv (see file signatures) 0xBEEF
Meta Size (4 bytes) [uint32]: The size of the metadata payload
Meta ByteString (meta_size bytes): A msgpack metadata object containing the info specified in Header Format
A series of repeating Frame Sequences. Each frame constitutes a contiguous block of bytes which can be read in as individual frames

## Header Format

| Property  | Num Bytes | Type | Notes |
| ------------- | ------------- | -------------| -------------|
| numFrames  | 4  | uint32 | All the frames in the file |
| fps  | 2  | uint16 | |




## Frame Sequence format
Each frame sequence has have the following structure:

1. Frame start marker (1 byte) [uint8]: This could be any unique random byte to just be like "hey, you're not crazy, this is where the frame starts"
1. Frame type (1 byte) [uint8]: 0 for IFrame and 1 for PFrame
1. Frame size (4 bytes) [uint32]: The size of all the frame bytes
1. Frame data (frame_size bytes): The actual frame data. This could either be:
    1. An IFrame, raw bytes from an glTF GLB file
    1. A PFrame, msgpack JSON diff object represented using the same encoding as the metadata header.

## Example

Draw this out in <<<<Fill this in>>>>>


