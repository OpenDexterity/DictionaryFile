# Block Table

Dictionaries are laid out in sections called blocks. Some blocks are dedicated to certain tasks like block 1 being the module table, block 2 being module names, and block 3 being the block table itself.

Blocks are **not** numbered by where they appear in the file, instead they are numbered by their index in the block table. For instance, block 2 could be placed in the file before block 1 and block 2 would still be block 2 as long as its record is 2nd in the block table.

The block table does not have a header for itself. All you need to know is the location and size of the table from the dictionary's header.

# Default Records

256 records are allocated by default in a blank dictionary, and there will always be three blocks in the same position with the same types every time. Their offsets and sizes are usually different between each file.

See block documentation for more info about block types.

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