# Module Structure

In a dictionary, resources need a different structure than regular blocks, so modules are used instead. Modules have more metadata than a block and can also use multiple blocks for each module-specific purpose.

Each module has three blocks: a name block, a data block, and a directory block.

Name blocks store the names of each resource inside that module. Modules themselves can have names as well. The module's own name is stored in block 2 at a specific offset that's specified in the module table.

Data blocks store the actual resource data, like script source code, script documentation, constants, globals, etc. Each resource type gets its own data block.

Directory blocks contain information about the actual resources that are stored inside of a module. They contain two tables and a header. The header contains the module's ID, and how many type slots and resource slots are allocated. 

The first table in a directory block is the type table, which contains a list of resource types and how many resources have that type inside the module. Since each type has its own set of resource IDs, you first have to find the resource type that you want to access. From there, you can see how many resources are allocated for that specific type, as well as where the slots are on the resource table. **One record in the type table can have multiple resources associated with it, but a resource can only have one type.**

The second table is the resource table. It contains information that's unique to a specific resource like the resource's name block, data block, and resource ID.

The header comes first in a directory block, then the type table, then the resource table. The type table should always be at offset 14 (0xE) in the block. To calculate the resource table offset, add the used type slot count with the unused type slot count, multiply by 16, then add 14.

Pseudocode example:
    
    uint ResourceTableOffset = ((UsedTypes + UnusedTypes) * 16) + 14;

# Module Table Header

The module table header is 9 bytes in size. Offset is from the beginning of the module table block. The unknown field in this instance might be the block number for the module names block but I haven't found any evidence to support that theory.

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Used Module Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td rowspan="2">Unused Module Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr><td>03</td></tr>

<tr>
<td>04</td> <td rowspan="4" colspan="2">Unknown</td>
</tr>
<tr><td>05</td></tr>
<tr><td>06</td></tr>
<tr><td>07</td></tr>

</table>

# Module Table Records

Module table records are 16 (0x10) bytes in size. Offset is from the beginning of the record. The name offset and name length refer to block 2, which is for the name of the module itself.

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Module Type</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td rowspan="4">Module ID</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>03</td></tr>
<tr><td>04</td></tr>
<tr><td>05</td></tr>

<tr>
<td>06</td> <td rowspan="4">Directory Block Number</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>07</td></tr>
<tr><td>08</td></tr>
<tr><td>09</td></tr>

<tr>
<td>0A</td> <td rowspan="4">Name Offset</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>0B</td></tr>
<tr><td>0C</td></tr>
<tr><td>0D</td></tr>

<tr>
<td>0E</td> <td rowspan="2">Name Length</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>0F</td>
</tr>

</table>

# Module Directory Header

The module directory block header is 14 (0xE) bytes in size. Offset is from the beginning of the directory block.

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Module Type</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td rowspan="4">Module ID</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>03</td></tr>
<tr><td>04</td></tr>
<tr><td>05</td></tr>

<tr>
<td>06</td> <td rowspan="2">Used Type Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr><td>07</td></tr>

<tr>
<td>08</td> <td rowspan="2">Unused Type Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr><td>09</td></tr>

<tr>
<td>0A</td> <td rowspan="2">Allocated Resource Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr><td>0B</td></tr>

<tr>
<td>0C</td> <td rowspan="2">Unallocated Resource Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr><td>0D</td></tr>

</table>

# Module Directory Type Table Records

Module directory type table records are 16 (0x10) bytes in size. Offset is from the beginning of the record. "First Resource Slot" is the index of the first record in the resource table that's associated with this type.

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Resource Type</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td rowspan="4">Name Block Number</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>03</td></tr>
<tr><td>04</td></tr>
<tr><td>05</td></tr>

<tr>
<td>06</td> <td rowspan="4">Data Block Number</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>07</td></tr>
<tr><td>08</td></tr>
<tr><td>09</td></tr>

<tr>
<td>0A</td> <td rowspan="2">First Resource Slot</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>0B</td>
</tr>

<tr>
<td>0C</td> <td rowspan="2">Used Resource Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>0D</td>
</tr>

<tr>
<td>0E</td> <td rowspan="2">Unused Resource Count</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>0F</td>
</tr>

</table>

# Module Directory Resource Table Records

Module directory resource table records are 16 (0x10) bytes in length. Offset is from the beginning of the record. The version field's purpose is unknown, but that's what it's called by Dex Utils. Name/data length and name/data offset refer to the name/data block that's defined in the types table (see above section).

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Resource ID</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td>Version</td> <td>Byte</td>
</tr>

<tr>
<td>03</td> <td>Name Length</td> <td>Byte</td>
</tr>

<tr>
<td>04</td> <td rowspan="4">Name Offset</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>05</td></tr>
<tr><td>06</td></tr>
<tr><td>07</td></tr>

<tr>
<td>08</td> <td rowspan="4">Data Offset</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>09</td></tr>
<tr><td>0A</td></tr>
<tr><td>0B</td></tr>

<tr>
<td>0C</td> <td rowspan="4">Data Length</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>0D</td></tr>
<tr><td>0E</td></tr>
<tr><td>0F</td></tr>

</table>