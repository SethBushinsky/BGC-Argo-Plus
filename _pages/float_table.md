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

<div class="datatable-begin"></div>

Food    | Description                           | Category | Sample type
------- | ------------------------------------- | -------- | -----------
Apples  | A small, somewhat round ...           | Fruit    | Fuji
Bananas | A long and curved, often-yellow ...   | Fruit    | Snow
Kiwis   | A small, hairy-skinned sweet ...      | Fruit    | Golden
Oranges | A spherical, orange-colored sweet ... | Fruit    | Navel

<div class="datatable-end"></div>

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


<script>
  $(document).ready(function() {
    $('#my-table').DataTable();
  });
</script>
