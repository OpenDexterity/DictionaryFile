# Module Structure

In a dictionary, resources need a different structure than regular blocks, so modules are used instead. Modules have more metadata than a block and can also use multiple blocks for each module-specific purpose.

Modules have three blocks associated with them: name blocks, data blocks, and directory blocks.

Name blocks store the names of each resource inside that module. Modules themselves can have names as well. The module's own name is stored in block 2 at a specific offset that's specified in the module table.

Data blocks store the actual resource data, like script source code, script documentation, constants, globals, etc. Each resource type gets its own data block.