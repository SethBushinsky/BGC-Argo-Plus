---
permalink: /test_table_3/
title: "Table"
author_profile: false
last_modified_at: 2025-06-12
toc: false

---
<table id="my-table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>30</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>25</td>
    </tr>
  </tbody>
</table>

{% raw %}
<script>
  $(document).ready(function () {
    $('#my-table').DataTable();
  });
</script>
{% endraw %}