# Module Structure

In a dictionary, resources need a different structure than regular blocks, so modules are used instead. Modules have more metadata than a block and can also use multiple blocks for each module-specific purpose.

Each module has three blocks: a name block, a data block, and a directory block.

Name blocks store the names of each resource inside that module. Modules themselves can have names as well. The module's own name is stored in block 2 at a specific offset that's specified in the module table.

Data blocks store the actual resource data, like script source code, script documentation, constants, globals, etc. Each resource type gets its own data block.

Directory blocks contain information about the actual resources that are stored inside of a module. They contain two tables and a header. The header contains the module's ID, and how many type slots and resource slots are allocated. 

The first table in a directory block is the type table, which contains a list of resource types and how many resources have that type inside the module. Since each type has its own set of resource IDs, you first have to find the resource type that you want to access. From there, you can see how many resources are allocated for that specific type, as well as where the slots are on the resource table. **One record in the type table can have multiple resources associated with it, but a resource can only have one type.**

The second table is the resource table. It contains information that's unique to a specific resource like the resource's name block, data block, and resource ID.

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