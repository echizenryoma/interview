## 数据库概论

### 范式

#### 第一范式

符合第一范式的关系中的每个属性都不可再分。

不符合第一范式的实例：

<table>
<tr>
    <td align="center" rowspan="2"><b>编号</b></td>
    <td align="center" rowspan="2"><b>品名</b></td>
    <td align="center" colspan="2"><b>进货</b></td>
    <td align="center" colspan="2"><b>销售</b></td>
    <td align="center" rowspan="2"><b>备注</b></td>
</tr>
<tr>
    <td align="center"><b>数量</b></td>
    <td align="center"><b>单价</b></td>
    <td align="center"><b>数量</b></td>
    <td align="center"><b>单价</b></td>
</tr>
<tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
</tr>
</table>

#### 第二范式

消除了非主属性对于码的部分函数依赖

码：设K为某表中的一个属性或属性组，若除K之外的所有属性都完全函数依赖于K，那么称K为候选码，简称为码。在实际中我们通常可以理解为：假如当K确定的情况下，该表除K之外的所有属性的值也就随之确定，那么K就是码。
