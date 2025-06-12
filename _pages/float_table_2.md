---
permalink: /float_table/
title: "Float information table "
excerpt: "Float Table"
author_profile: false
last_modified_at: 2025-06-12
classes: wide
toc: false

<!-- Datatable plugin CSS file -->
    <link rel="stylesheet" href=
"https://cdn.datatables.net/1.10.22/css/jquery.dataTables.min.css" />

     <!-- jQuery library file -->
     <script type="text/javascript" 
         src="https://code.jquery.com/jquery-3.5.1.js">
     </script>

      <!-- Datatable plugin JS library file -->
     <script type="text/javascript" src=
"https://cdn.datatables.net/1.10.22/js/jquery.dataTables.min.js">
     </script>

---

<body>
    <h2>load data from JavaScript array using Datatables</h2>

    <!--HTML table with student data-->
    <table id="tableID" class="display" style="width:100%">
        <thead>
            <tr>
                <th>User name</th>
                <th>Designation</th>
                <th>Salary</th>
                <th>Address</th>
                <th>City</th>                
            </tr>
        </thead>
    </table>

    <script>
    var dataSet = [
    [
    
      "Tina Mukherjee",
       "BPO member",
       "300000",
        "24,chandni chowk",
        "Pune"    
    ],
    [
     "Gaurav",
      "Teacher",
      "100750",
      "esquare,JM road",
      "Pune"     
    ],
    [
       "Ashtwini",     
         "Junior engineer",
         "860000",  
        "Santa cruz",
        "mumbai"    
    ],
    [
         "Celina",     
         "Javascript Developer",
         "430060",
         "crr lake side ville",
         "tellapur"     
    ],
    [
        "Aisha",     
         "Nurse",
         "160000",
        "rk puram",
        "Delhi"     
    ],
    [
        "Brad henry",     
         "Accountant",
         "370000",  
        "chaurasi lane",
        "Kolkatta"     
    ],
    [
         "Harry",    
         "Salesman",
         "130500",
         "32, krishna nagar",
         "Navi mumbai"    
    ],
    [
      "Rhovina",
      "Amazon supporter",
      "300900",    
      "Aparna zone",
      "hyderabad"
     ],
    [
      "Celina",
      "Senior Developer",
      "200000",
      "23, chandni chowk",
      "pune"  
    ],     
    [
      "Glenny",
      "Administrator",
       "200500",
       "Nagpur",
       "Maharashtra"    
    ],
    [
      "Brad Pitt",
       "Engineer",
       "100000",
        "sainikpuri",
        "Delhi"  
    ],
    [
         "Deepa",    
         "Team Leader",
         "200500",     
        "Annanagar",
        "Chennai"    
    ],
   
    [
      "Angelina",     
         "CEO",
         "1000000",   
        "JM road",
        "Aundh pune"
      
    ]
 ];

    $(document).ready(function() {
        $('#tableID').DataTable( {
            data: dataSet,
            columns: [
                { title: "Name" },
                { title: "Designation" },
                { title: "Salary" },
                { title: "Address" },
                { title: "City" }       
                
            ]
        } );
    } );
</script>
</body>

</html>