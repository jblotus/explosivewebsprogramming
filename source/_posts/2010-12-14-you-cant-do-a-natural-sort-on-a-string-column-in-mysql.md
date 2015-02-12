---
title: 'You cant do a natural sort on a string column in MySQL'
author: jblotus
layout: post
dsq_thread_id:
  - 312027360
categories:
  - MySQL
  - SQL
tags:
  - mysql
  - natural sort
  - natural sorting
  - sql
---
Recently a question popped up over on the [CakePHP google group][1] where someone had a problem sorting on a column of "registration numbers". This was his table sorted order by "**Reg No ASC**":

<table class="table">
  <tr>
    <td>
      ID
    </td>

    <td>
      Name
    </td>

    <td>
      Reg No
    </td>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      Product A
    </td>

    <td>
      1
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      Product B
    </td>

    <td>
      123
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      Product C
    </td>

    <td>
      2
    </td>
  </tr>
</table>

1, 123, 2. That's not correct right? **Well it is if we are sorting strings!** The problem here is that [MySQL will not do a "**natural sort**"][2] on a text type column like *char*, *varchar*, or *text*. The solution to this is to change the column type to a numeric value, especially considering he is storing numbers anyway! Once you change "**Reg No**" to *int*, the column will sort correctly.

<table class="table">
  <tr>
    <td>
      ID
    </td>

    <td>
      Name
    </td>

    <td>
      Reg No
    </td>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      Product A
    </td>

    <td>
      1
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      Product C
    </td>

    <td>
      2
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      Product B
    </td>

    <td>
      123
    </td>
  </tr>
</table>

Always remember to check your schema!

 [1]: http://groups.google.com/group/cake-php/browse_thread/thread/5c6440a91795ec2d#
 [2]: http://stackoverflow.com/questions/153633/natural-sort-in-mysql
