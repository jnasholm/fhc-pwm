<h1>ESP32 pin schema</h1>

The controller IO pins are assigned according to the table below.
<table>
  <tr><th>Function<th>Pin number<th>Comment</tr>
  <tr><td>Zone 1 actuator<td>16<td></tr>
  <tr><td>Zone 2 actuator<td>17<td>Used for ethernet port if enabled</tr>
  <tr><td>Zone 3 actuator<td>18<td>Used for ethernet port if enabled</tr>
  <tr><td>Zone 4 actuator<td>19<td></tr>
  <tr><td>Zone 5 actuator<td>22<td></tr>
  <tr><td>Zone 6 actuator<td>23<td>Used for ethernet port if enabled</tr>
  <tr><td>Zone 7 actuator<td>25<td></tr>
  <tr><td>Zone 8 actuator<td>26<td></tr>
  <tr><td>Mixing valve actuator decrease<td>13<td></tr>
  <tr><td>Mixing valve actuator increase<td>14<td></tr>
  <tr><td>Room temperature sensors (DS18B20 or RHT03)<td>21<td></tr>
  <tr><td>Distributor line temperature sensors (DS18B20)<td>27<td></tr>
  <tr><td>System status LED<td>32<td></tr>
  <tr><td>Controller status LED<td>33<td></tr>  
  <tr><td>Battery voltage<td>35<td>May require soldering on the ESP32 board to enable</tr>
  <tr><td>Charging voltage<td>39<td>May require soldering on the ESP32 board to enable</tr>
</table>
