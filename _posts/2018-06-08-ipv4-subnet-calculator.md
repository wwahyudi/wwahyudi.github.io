---
layout:     post
title:      IPV4 Subnet Calculator
date:       2018-06-08
summary:   	IPV4 Subnet Calculator
categories: network
---

<script language="javascript" type="text/javascript">
<!--

var curNetwork = 0;
var curMask = 0;

function updateNetwork()
{
  var newNetworkStr = document.forms['calc'].elements['network'].value;
  var newMask = parseInt(document.forms['calc'].elements['netbits'].value);

  var newNetwork = inet_aton(newNetworkStr);

  if (newNetwork === null) {
    alert('Invalid network address entered');
    return;
  }

  var tmpNetwork = network_address(newNetwork, newMask);
  if (newNetwork != tmpNetwork) {
    alert('The network address entered is not on a network boundary for this mask.\nIt has been changed to '+inet_ntoa(tmpNetwork)+'.');
    newNetwork = tmpNetwork;
    document.forms['calc'].elements['network'].value = inet_ntoa(tmpNetwork);
  }

  if (newMask < 0 || newMask > 32) {
    alert('The network mask you have entered is invalid');
    return;
  }

  if (curMask == 0) {
    curMask = newMask;
    curNetwork = newNetwork;
    startOver();
  }
  else if (curMask != newMask && confirm('You are changing the base network from /'+curMask+' to /'+newMask+'. This will reset any changes you have made. Proceed?')) {
    curMask = newMask;
    curNetwork = newNetwork;

    startOver();
  }
  else {
    document.forms['calc'].elements['netbits'].value = curMask;
    curNetwork = newNetwork;

    recreateTables();
  }
}

function startOver()
{
  rootSubnet = [0, 0, null];

  recreateTables();
}

function recreateTables()
{
  var calcbody = document.getElementById('calcbody');
  if (!calcbody) {
    alert('Body not found');
    return;
  }

  while (calcbody.hasChildNodes()) {
    calcbody.removeChild(calcbody.firstChild);
  }

  updateNumChildren(rootSubnet);
  updateDepthChildren(rootSubnet);

  createRow(calcbody, rootSubnet, curNetwork, curMask, [curMask, rootSubnet[1], rootSubnet], rootSubnet[0]);

  document.getElementById('joinHeader').colSpan = (rootSubnet[0] > 0 ? rootSubnet[0] : 1);
  document.getElementById('col_join').span = (rootSubnet[0] > 0 ? rootSubnet[0] : 1);

}

function nodeToString(node)
{
  if (node[2]) {
    return '1'+nodeToString(node[2][0])+nodeToString(node[2][1]);
  }
  else {
    return '0';
  }
}

function binToAscii(str)
{
  var curOut = '';
  var curBit = 0;
  var curChar = 0;

  for (var i=0; i<str.length; i++) {
    if (str.charAt(i) == '1') {
      curChar |= 1<<curBit;
    }
    curBit++;
    if (curBit > 3) {
      curOut += curChar.toString(16);
      curChar = 0;
      curBit = 0;
    }
  }
  if (curBit > 0) {
    curOut += curChar.toString(16);
  }
  return str.length+'.'+curOut;
}

function asciiToBin(str)
{
  var re = /([0-9]+)\.([0-9a-f]+)/;
  var res = re.exec(str);
  var len = res[1];
  var encoded = res[2];
  var out = '';
  for (var i=0; i< res[1]; i++) {
    var ch = parseInt(res[2].charAt(Math.floor(i/4)), 16);
    var pos = i % 4;
    out += (ch & (1<<pos) ? '1' : '0');
  }
  return out;
}

function createRow(calcbody, node, address, mask, labels, depth)
{
  if (node[2]) {
    var newlabels = labels;
    newlabels.push(mask+1);
    newlabels.push(node[2][0][1]);
    newlabels.push(node[2][0]);
    createRow(calcbody, node[2][0], address, mask+1, newlabels, depth-1);

    newlabels = new Array();
    newlabels.push(mask+1);
    newlabels.push(node[2][1][1]);
    newlabels.push(node[2][1]);
    createRow(calcbody, node[2][1], address+subnet_addresses(mask+1), mask+1, newlabels, depth-1);
  }
  else {
    var newRow = document.createElement('TR');
    calcbody.appendChild(newRow);

    /* subnet address */
    var newCell = document.createElement('TD');
    newCell.appendChild(document.createTextNode(inet_ntoa(address)+'/'+mask));
    newRow.appendChild(newCell);

    var addressFirst = address;
    var addressLast = subnet_last_address(address, mask);
    var useableFirst = address + 1;
    var useableLast = addressLast - 1;
    var numHosts;
    var addressRange;
    var usaebleRange;

    if (mask == 32) {
      addressRange = inet_ntoa(addressFirst);
      useableRange = addressRange;
      numHosts = 1;
    }
    else {
      addressRange = inet_ntoa(addressFirst)+' - '+inet_ntoa(addressLast);
      if (mask == 31) {
	useableRange = addressRange;
	numHosts = 2;
      }
      else {
	useableRange = inet_ntoa(useableFirst)+' - '+inet_ntoa(useableLast);
	numHosts = (1 + useableLast - useableFirst);
      }
    }

    /* netmask */
    var newCell = document.createElement('TD');
    newCell.appendChild(document.createTextNode(inet_ntoa(subnet_netmask(mask))));
    newRow.appendChild(newCell);

    /* range of addresses */
    var newCell = document.createElement('TD');
    newCell.appendChild(document.createTextNode(addressRange));
    newRow.appendChild(newCell);

    /* useable addresses */
    var newCell = document.createElement('TD');
    newCell.appendChild(document.createTextNode(useableRange));
    newRow.appendChild(newCell);

    /* Hosts */
    var newCell = document.createElement('TD');
    newCell.appendChild(document.createTextNode(numHosts));
    newRow.appendChild(newCell);

    /* actions */

    var newCell = document.createElement('TD');
    newRow.appendChild(newCell);

    if (mask == 32) {
      var newLink = document.createElement('SPAN');
      newLink.className = 'disabledAction';
      newLink.appendChild(document.createTextNode('Divide'));
      newCell.appendChild(newLink);
    }
    else {
      var newLink = document.createElement('span');
       newLink.className = 'actionButton';
      newLink.onclick = function () { divide(node); return false; }
      newLink.appendChild(document.createTextNode('Divide'));
      newCell.appendChild(newLink);
    }

    var colspan = depth - node[0];

    for (var i=(labels.length/3)-1; i>=0; i--) {
      var mask = labels[i*3];
      var rowspan = labels[(i*3)+1];
      var joinnode = labels[(i*3)+2];

      var newCell = document.createElement('TD');
      newCell.rowSpan = (rowspan > 1 ? rowspan : 1);
      newCell.colSpan = (colspan > 1 ? colspan : 1);

      var newPrefix = document.createElement('span');
      newPrefix.appendChild(document.createTextNode('/'+mask));
      if (i == (labels.length/3)-1) {
	      newCell.className = 'maskSpan';
      }
      else {
        
        newCell.className = 'maskSpanJoinable';
        newCell.onclick = newJoin(joinnode);
        newPrefix.className = 'actionButton';
        
	      //newCell.onmouseover = function() { window.status = joinnode[0]+'---'+joinnode[1]+'---'+joinnode[2]+'>>>>>'+node[2];}
      }

      newCell.appendChild(newPrefix);
      newRow.appendChild(newCell);


      colspan = 1; // reset for subsequent cells
    }
  }

}

/* This is necessary because 'joinnode' changes during the scope of the caller */
function newJoin(joinnode)
{
  return function() { join(joinnode) };
}

function divide(node)
{
  node[2] = new Array();
  node[2][0] = [0, 0, null];
  node[2][1] = [0, 0, null];
  recreateTables();
}

function join(node)
{
  /* easy as pie */
  node[2] = null;
  recreateTables();
}

function updateNumChildren(node)
{
  if (node[2] == null) {
    node[1] = 0;
    return 1;
  }
  else {
    node[1] = updateNumChildren(node[2][0]) + updateNumChildren(node[2][1]);
    return node[1];
  }
}

function updateDepthChildren(node)
{
  if (node[2] == null) {
    node[0] = 0;
    return 1;
  }
  else {
    node[0] = updateDepthChildren(node[2][0]) + updateDepthChildren(node[2][1]);
    return node[1];
  }
}


var rootSubnet;

// each node is Array:
// [0] => depth of children, total number of visible children, children


function inet_ntoa(addrint)
{
  return ((addrint >> 24) & 0xff)+'.'+
    ((addrint >> 16) & 0xff)+'.'+
    ((addrint >> 8) & 0xff)+'.'+
    (addrint & 0xff);
}

function inet_aton(addrstr)
{
  var re = /^([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})$/;
  var res = re.exec(addrstr);

  if (res === null) {
    return null;
  }

  for (var i=1; i<=4; i++) {
    if (res[i] < 0 || res[i] > 255) {
      return null;
    }
  }

  return (res[1] << 24) | (res[2] << 16) | (res[3] << 8) | res[4];
}

function network_address(ip, mask)
{
  var maskbits = 0;
  for (var i=31-mask; i>=0; i--) {
    ip &= ~ 1<<i;
  }
  return ip;
}

function subnet_addresses(mask)
{
  return 1<<(32-mask);
}

function subnet_last_address(subnet, mask)
{
  return subnet + subnet_addresses(mask) - 1;
}

function subnet_netmask(mask)
{
  return network_address(0xffffffff, mask);
}

function calcOnLoad()
{
  args = parseQueryString();
  if (args['network'] && args['mask'] && args['division']) {
    document.forms['calc'].elements['network'].value = args['network'];
    document.forms['calc'].elements['netbits'].value = args['mask'];
    updateNetwork();
    var division = asciiToBin(args['division']);
    rootSubnet = [0, 0, null];
    if (division != '0') {
      loadNode(rootSubnet, division);
    }
    recreateTables();
  }
  else {
    updateNetwork();
  }
}

function loadNode(curNode, division)
{
  if (division.charAt(0) == '0') {
    return division.substr(1);
  }
  else {
    curNode[2] = new Array();
    curNode[2][0] = [0, 0, null];
    curNode[2][1] = [0, 0, null];

    division = loadNode(curNode[2][0], division.substr(1));
    division = loadNode(curNode[2][1], division);
    return division;
  }
}

function parseQueryString (str)
{
  str = str ? str : location.search;
  var query = str.charAt(0) == '?' ? str.substring(1) : str;
  var args = new Object();
  if (query) {
    var fields = query.split('&');
    for (var f = 0; f < fields.length; f++) {
      var field = fields[f].split('=');
      args[unescape(field[0].replace(/\+/g, ' '))] = 
	unescape(field[1].replace(/\+/g, ' '));
    }
  }
  return args;
}

window.onload = calcOnLoad;

function toggleColumn(cb)
{
  var colName = 'col_'+(cb.id.substr(3));
  var col = document.getElementById(colName);

  if (cb.checked) {
    col.style.display = 'block';
  }
  else {
    col.style.display = 'none';
  }
  recreateTables(); /* because IE draws lines all over the place with border-collapse */
}

//-->
</script>

<style type="text/css">

p.subnet {
  font-size: 75%;
}

.noborder, .noborder tr, .noborder th, .noborder td { 
  border: none; 
}

.btn {
  background: #3498db;
  background-image: -webkit-linear-gradient(top, #3498db, #2980b9);
  background-image: -moz-linear-gradient(top, #3498db, #2980b9);
  background-image: -ms-linear-gradient(top, #3498db, #2980b9);
  background-image: -o-linear-gradient(top, #3498db, #2980b9);
  background-image: linear-gradient(to bottom, #3498db, #2980b9);
  -webkit-border-radius: 12;
  -moz-border-radius: 12;
  border-radius: 12px;
  font-family: Arial;
  color: #ffffff;
  font-size: 20px;
  padding: 10px 20px 10px 20px;
  border: solid #1f628d 2px;
  text-decoration: none;
}

.btn:hover {
  background: #3cb0fd;
  background-image: -webkit-linear-gradient(top, #3cb0fd, #3498db);
  background-image: -moz-linear-gradient(top, #3cb0fd, #3498db);
  background-image: -ms-linear-gradient(top, #3cb0fd, #3498db);
  background-image: -o-linear-gradient(top, #3cb0fd, #3498db);
  background-image: linear-gradient(to bottom, #3cb0fd, #3498db);
  text-decoration: none;
}

.btn:active {
  background: #3498db;
}

.label {
  font-size: 60%;
}

.calc {
  font-family: Arial, Verdana, sans-serif;
  border-collapse: collapse; 
}
.calc td {
  border: 1px solid black;
}

.calc thead {
  font-weight: bold;
  background-color: #eeeeee;
  font-size: 60%;
  border: 1px solid black;
}

#calcbody {
  font-size: 55%;
}

.disabledAction {
  color: #dddddd;
}


.actionButton {
  font: bold;
  text-decoration: none;
  background-color: #EEEEEE;
  color: #000000;
  padding: 2px;
  border-top: 1px solid #CCCCCC;
  border-right: 1px solid #333333;
  border-bottom: 1px solid #333333;
  border-left: 1px solid #CCCCCC;
  cursor: pointer;
} 

.actionButton:hover{

  text-decoration: none;
  
  border-top: 1px solid #CCCCCC;
  border-right: 1px solid #333333;
  border-bottom: 1px solid #333333;
  border-left: 1px solid #CCCCCC;
}

.maskSpan {
  color: red;
}
.maskSpanJoinable {
  background-color: #cccccc;
}

.source {
  font-size: 0.5em;
}
</style>

<table class="noborder">
  <tr>
    <td>
      
      <p class="noborder">Enter the network you wish to subnet:</p>
      <form name="calc" onsubmit="updateNetwork(); return false;">
        <table cellspacing="0" class="noborder" >
          <tr>
            <td class="label">Network Address / Mask bits</td>
            <td class="label"></td>
          </tr>
          <tr>
            <td>
              <input type="text" name="network" size="15" maxlength="15" value="192.168.0.0">/<input type="text" name="netbits" size="2" maxlength="2" value="24"></td>
            <td>
              <input class="btn" type="submit" value="Update">
              <input class="btn" type="button" value="Reset" onclick="if (confirm('This will reset all subnet divisions you have made. Proceed?')) startOver();">
            </td>
          </tr>
        </table>
      </form>
    </td>
  </tr>
</table>

<hr >

<table class="calc" cellspacing="0" cellpadding="2">
  <colgroup>
    <col id="col_subnet">
    <col id="col_netmask" style="display: none">
    <col id="col_range">
    <col id="col_useable">
    <col id="col_hosts">
    <col id="col_divide">
    <col id="col_join">
  </colgroup>
  <thead>
    <tr>
      <td>Subnet address</td>
      <td>Netmask</td>
      <td>Range of addresses</td>
      <td>Useable IPs</td>
      <td>Hosts</td>
      <td>Divide</td>
      <td id="joinHeader">Join</td>
    </tr>
  </thead>
  <tbody id="calcbody">
    <!--tr>
      <td>130.94.203.0/24</td>
      <td>130.94.203.0 - 130.94.203.255</td>
      <td>130.94.203.1 - 130.94.203.254 (254)</td>
      <td>Divide</td>
    </tr-->
  </tbody>
</table>  

<br>
<br>
<p class="source">Source code taken from <a href="https://github.com/davidc/subnets" target="_blank">davidc/subnets</a></p>