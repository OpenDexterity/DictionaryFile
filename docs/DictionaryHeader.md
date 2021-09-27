# Dictionary Header

Most file formats have a header that contains some basic information, and this format is no exception. Dictionary file headers have: a signature at the very beginning, the size of the file, where the block table is stored, and how many records are in the block table.

# Values

Header size is 26 (0x1A) bytes. Signature is assumed to be ASCII characters. Offset is from the beginning of the file.

<table>

<thead>
<tr>
<th>Offset (hex)</th> <th>Purpose</th> <th>Data type</th>
</tr>
</thead>

<tbody>

<tr>
<td>00</td> <td rowspan="4">Signature</td> <td rowspan="4">ASCII string</td>
</tr>
<tr><td>01</td></tr>
<tr><td>02</td></tr>
<tr><td>03</td></tr>

<tr>
<td>04</td> <td rowspan="4" colspan="2">Unknown</td>
</tr>
<tr><td>05</td></tr>
<tr><td>06</td></tr>
<tr><td>07</td></tr>

<tr>
<td>08</td> <td rowspan="4">OS File Size On Disk</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>09</td></tr>
<tr><td>0A</td></tr>
<tr><td>0B</td></tr>

<tr>
<td>0C</td> <td rowspan="2" colspan="2">Unknown</td>
</tr>
<tr><td>0D</td></tr>

<tr>
<td>0E</td> <td rowspan="4">Block table offset</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>0F</td></tr>
<tr><td>10</td></tr>
<tr><td>11</td></tr>

<tr>
<td>12</td> <td rowspan="4">Block table length</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>13</td></tr>
<tr><td>14</td></tr>
<tr><td>15</td></tr>

<tr>
<td>16</td> <td rowspan="4">Unallocated block count</td> <td rowspan="4">UInt32</td>
</tr>
<tr><td>17</td></tr>
<tr><td>18</td></tr>
<tr><td>19</td></tr>

</tbody>

</table>
