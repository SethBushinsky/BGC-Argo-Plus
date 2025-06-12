---
permalink: /float_table/
title: "Float information table "
excerpt: "Float Table"
author_profile: false
last_modified_at: 2025-06-12
classes: wide
toc: false
datatable: true
---

<table id="Table1" class="display">
    <thead>
        <tr>
            <th>Column 1</th>
            <th>Column 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Row 1 Data 1</td>
            <td>Row 1 Data 2</td>
        </tr>
        <tr>
            <td>Row 2 Data 1</td>
            <td>Row 2 Data 2</td>
        </tr>
    </tbody>
</table>


<table id="my-table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>30</td>
      <td>Engineer</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>25</td>
      <td>Designer</td>
    </tr>
  </tbody>
</table>

$(document).ready( function () {
    $('#Table1').DataTable();
} );


{% raw %}
<script>
  $(document).ready(function () {
    $('#my-table').DataTable();
  });
</script>
{% endraw %}