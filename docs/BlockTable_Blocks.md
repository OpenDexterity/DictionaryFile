# Block Table

Dictionaries are laid out in sections called blocks. Some blocks are dedicated to certain tasks like block 1 being the module table, block 2 being module names, and block 3 being the block table itself.

Blocks are **not** numbered by where they appear in the file, instead they are numbered by their index in the block table. For instance, block 2 could be placed in the file before block 1 and block 2 would still be block 2 as long as its record is 2nd in the block table.

The block table does not have a header for itself. All you need to know is the location and size of the table from the dictionary's header.

# Default Records

256 records are allocated by default in a blank dictionary, and there will always be three blocks in the same position with the same types every time. Their offsets and sizes are usually different between each file.


<table>

<tr>
<th>Block #</th> <th>Type</th> <th>Purpose</th>
</tr>

<tr>
<td>1</td> <td>1</td> <td>Module Table</td>
</tr>

<tr>
<td>2</td> <td>3</td> <td>Module Names</td>
</tr>

<tr>
<td>3</td> <td>6</td> <td>Block Table</td>
</tr>

</table>

# Records

Records are 14 (0xE) bytes in size. Block number comes from the record's index in the table, starting at 1.

<table>

<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>

<tr>
<td>00</td> <td rowspan="2">Block Type</td> <td rowspan="2">UInt16</td>
</tr>
<tr>
<td>01</td>
</tr>

<tr>
<td>02</td> <td rowspan="4">Block Start Offset</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>03</td></tr>
<tr><td>04</td></tr>
<tr><td>05</td></tr>

<tr>
<td>06</td> <td rowspan="4">Block Size (bytes)</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>07</td></tr>
<tr><td>08</td></tr>
<tr><td>09</td></tr>

<tr>
<td>0A</td> <td rowspan="4">Unused Space (bytes)</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>0B</td></tr>
<tr><td>0C</td></tr>
<tr><td>0D</td></tr>

</table>

---

# Blocks

Everything in a dictionary (except the file header) is contained in a block. Blocks **are always** required to have an entry in the block table. 

Blocks can also have some blank space on the end that's allocated but not used yet. This blank space is marked by the Unused field in the block table. This blank space is *(assumed to be)* always on the end of the block. 

Entire blocks also can be blank space, and are marked as type 0 in the block table. Type 0 is also for blocks that aren't allocated at all. To distinguish between unused (but allocated) and unallocated blocks, check the block table. Unallocated blocks have all zeroes for values. Unused blocks have a size and an offset defined *(assumed)*.

# Block Types

Each block has a type associated with it. The type indicates what the block's contents are.

Have not been able to find anything that uses block type 5. The type value is a 16 bit integer so there theoretically could be 65,535 types.

<table>

<tr>
<th>Type</th> <th>Purpose</th>
</tr>

<tr>
<td>0</td> <td>Unallocated/Unused</td>
</tr>

<tr>
<td>1</td> <td>Module Table</td>
</tr>

<tr>
<td>2</td> <td>Module Directory</td>
</tr>

<tr>
<td>3</td> <td>Module Names</td>
</tr>

<tr>
<td>4</td> <td>Module Data</td>
</tr>

<tr>
<td>5</td> <td>Unknown</td>
</tr>

<tr>
<td>6</td> <td>Block Table</td>
</tr>

</table>