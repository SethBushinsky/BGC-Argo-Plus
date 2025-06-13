---
permalink: /float_table/
title: "Float information table "
author_profile: false
last_modified_at: 2025-06-12
toc: false
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/2.3.2/css/dataTables.dataTables.css">
<script type="text/javascript" charset="utf8" src="//cdn.datatables.net/2.3.2/js/dataTables.js"></script>
---


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

<div class="datatable-begin"></div>

Food    | Description                           | Category | Sample type
------- | ------------------------------------- | -------- | -----------
Apples  | A small, somewhat round ...           | Fruit    | Fuji
Bananas | A long and curved, often-yellow ...   | Fruit    | Snow
Kiwis   | A small, hairy-skinned sweet ...      | Fruit    | Golden
Oranges | A spherical, orange-colored sweet ... | Fruit    | Navel

<div class="datatable-end"></div>

