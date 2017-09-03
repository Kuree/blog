---
published: false
---
## A IO Library for Typescript

### TL;DR
This is a endianess-aware binary reader and writer for Typescript. You can use it to read arbitrary binary files in browser. You can see the live demo [here](https://keyizhang.com/ts-sims4) and a simple example [here](#usage). 

### Origins
C#'s ```BinaryReader``` and ```BinaryWriter``` are one of my favorite IO interface. They are simple, intuitive, and easy-to-use. I have had the chance to develop some interesting tools using these two classes, for instance, S4PE for The Sims 4. Maxis used their own serialization protocols to parse and un-parse their data files. As a result, you have to parse the files by hand, that it, specifying where and when to read an int, float, etc. 

Yet web technology is catching up and there is a trend to move desktop applications to the web. One big issue with Sims 4 modding is that modders have to stick to Windows platform because most of the tools are targeted to it. This phenomenon also applies to many games.

What if, let's just image, all the modding can be done in the browser? Does that mean users are free of any platform limitations as long as they are using an updated browser? My answer to that is yes, and here is my implementation to the binary package.

### Usage
There are several ways to load the library. The current setup is to use ```require.js``` to load the necessary package. Here is a example of how to read a Sims DBPF file.
```typescript
private readHeader(data: Blob | Uint8Array): number {
    
    var r = new IO.BinaryReader(data);
    var fourcc = r.readString(4);

    this.Major = r.readInt32();
    this.Minor = r.readInt32();
}

```

Due to the limitation of Jacascript, ```Uint64``` cannot be represented perfectly in browser. The library uses a simple ```Uint64``` to support big number reading and writing. Currently there is no implementation for ```Int64```. Users can use other big number library when needed.

Below is the list of implemented interfaces for ```BinaryReader```.

- ```readInt8(): number ```
- ```readUInt8(): number```
- ```readInt16(): number``` 
- ```readUInt16(): number```
- ```readInt32(): number```
- ```readUInt32(): number```
- ```readUInt64(): Uint64```
- ```readFloat(): number```
- ```readDouble(): number```
- ```readBytes(size): Uint8Array```
- ```readChar(): string```
- ```readString(length): string```
- ```seek(pos): void```
- ```position(): number```

```BinaryWrter``` has similar interface as ```BinaryReader```.

To make handling IO easier, the IO library also has a ```BitConverter``` as in ```C#```. Here is a list of its interface.
- ```toUInt16(data: Uint8Array | Blob, offset: number): number ```
- ```toInt16(data: Uint8Array | Blob, offset: number): number```
- ```toUInt32(data: Uint8Array | Blob, offset: number): number```
- ```toInt32(data: Uint8Array | Blob, offset: number): number```


### How to install
For now it's part of the [```ts-sims4```](https://github.com/Kuree/ts-sims4/blob/master/src/io.ts) library. The easiest way to use is to copy the ```io.ts``` file to your working directory and use it in your Typescript project. I will make a ```npm``` package in the future.