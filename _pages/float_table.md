---
permalink: /float_table/
title: "Float information table "
author_profile: false
last_modified_at: 2025-06-12
toc: false

---

<div class="datatable-begin"></div>

Food    | Description                           | Category | Sample type
------- | ------------------------------------- | -------- | -----------
Apples  | A small, somewhat round ...           | Fruit    | Fuji
Bananas | A long and curved, often-yellow ...   | Fruit    | Snow
Kiwis   | A small, hairy-skinned sweet ...      | Fruit    | Golden
Oranges | A spherical, orange-colored sweet ... | Fruit    | Navel

<div class="datatable-end"></div>


<script>
$(document).ready(function(){
    $('div.datatable-begin').nextUntil('div.datatable-end', 'table').addClass('display');
    $('table.display').DataTable( {
        paging: true,
        stateSave: true,
        searching: true
    });
});
</script>