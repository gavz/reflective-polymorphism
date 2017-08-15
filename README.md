# Reflective Unloader
This is code that can can be used within a PE file to allow it to reflectively
reconstruct itself in memory at runtime. The result is a byte for byte copy of
the original PE file. This can be combined with Reflective DLL injection to
allow code to reconstruct itself after being loaded through an arbitrary means.

The original PE file will not be modified in memory, this code makes a new copy
of the unloaded target.

## Usage

1. Add `ReflectiveUnloader.c \ ReflectiveUnloader.h` to the desired project.
   Once added, call `ReflectiveUnloader()` with a handle to the module to unload
   and reconstruct.
1. After compiling the project, run `pe_patch.py` to patch in necessary data to
   the pe file. Without this step, the writable sections of the PE file will be
   corrupted in the unloaded copy.

## PE Patching
It's necessary to patch the PE file to get a perfect byte-for-byte copy when it
is unloaded. The patching process creates a new `.restore` section where a copy
of any writable sections are backed up. When the `ReflectiveUnloader` function
is then called, it will process this extra section to restore the original
contents to the writable sections.

If the `.restore` section is not present, the unloader will simply skip this
step. This allows the unloader to perform the same task for arbitrary unpatched
PE files, however know that any modifications to segments made at runtime will
be present in the unloaded PE file.

## Credits

 - Brandan Geise - coldfusion ([@coldfusion39](https://twitter.com/coldfusion39))
 - Spencer McIntyre - zeroSteiner ([@zeroSteiner](https://twitter.com/zeroSteiner))
