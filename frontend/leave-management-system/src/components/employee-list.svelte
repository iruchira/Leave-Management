<script>
  import Icon from "@iconify/svelte";
  import { createEventDispatcher } from 'svelte';
  import { onMount } from "svelte";
  import createEmployee from "../controllers/manager";
  import { navigate } from "svelte-routing";
  //create a class instance//
  const listofEmp = new createEmployee();
  //create event dispatcher for update employee//
  const dispatch = createEventDispatcher();

  let managerData = JSON.parse(sessionStorage.getItem("data"));
  console.log(managerData, "managerData");
  let managerEmailId = managerData.email_id;
  console.log(managerEmailId, "manageremailid");

  //store result from services into array//
  let listArray = [];
  let totalRecordsInArray = "";
  let totalPages;
  let limit =5;
  // let recordsperPage;
  $: page = 1;

 const recordsperPage=(page)=> {
    const trimStart = (page-1)*limit
    const trimEnd = trimStart + limit
    return listArray.slice(trimStart, trimEnd);
}
  const getData =async()=>{
    try{
    const result = await listofEmp.getEmployeeList(managerEmailId, page);
    console.log(await result);
    listArray = result.data;
    totalRecordsInArray = listArray.length;
    totalPages = Math.ceil(totalRecordsInArray /limit);
    // recordsperPage = page * limit;
    // const trimStart = (page - 1) * limit;
    // const trimEnd = trimStart + limit;
    }catch(e){
      console.log("error while fetching data from server..");
    }
  }

//pagination//
        if (document.getElementById == "next" && page < totalRecordsInArray) {
            page++;
            getData();
        } else if (
            document.getElementById == "previous" &&
            page > 1 &&
            page <= totalRecordsInArray
        ) {
            page--;
            getData();
            console.log("prev pressed");
        } else if (page > 0 && page<= totalRecordsInArray) {
            page = page;
            getData();
        };

   const handlePrev = () => {
    if(page>0){
    page=page-1;
    }
    console.log("clicked previous");  
  };
  const handleNext = () => {
    if(page<totalPages.length){
      page=page+1;
    }
   console.log("clicked next");
  };
  const handlePage = (page) => {
    if (page === 0) {
      page = 1;
    } else {
      page = page;
    }
    console.log(page,"current page")
  };

//handle delete//
const handleDelete = async (id)=>{
  console.log(id,"delete id");
  const result = await listofEmp.deleteEmployee(id);
}  
  
const handleUpdate = (updateData)=>{
  //const id =emp.emp_id;
  dispatch("update",updateData);
  console.log(updateData);
}

  onMount(async () => {
    getData();
    // const trimStart = (page - 1) * limit;
    // const trimEnd = trimStart + limit;
    // totalCount = result.totalCount;
    // console.log(totalRecords);
    // totalRecordsInArray = Math.ceil(totalRecords/2);
    // console.log(totalRecordsInArray, "total counts related manager");
    // console.log(await listArray, "im list array");
  });
</script>

<div class="container-fluid">
  <div class="table-responsive">
    <div class="table-wrapper">
      <div class="table-title mt-2 mb-2">
        <div class="row">
          <div class="col-sm-8 mb-4" style="color:green">
            <h2>Employee <b>List</b></h2>
          </div>
          <div class="col-sm-4">
            <div class="search-box">
              <input type="text" class="form-control" placeholder="Search…" />
            </div>
          </div>
        </div>
      </div>
      <table class="table table-striped table-hover table-bordered">
        <thead>
          <tr>
            <th>#</th>
            <th>Name </th>
            <th>Email Id</th>
            <th>Department </th>
            <th>Designation</th>
            <th>Date of Joining </th>
            <th>Contact No</th>
            <th>Role</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          {#each listArray as emp}
            <tr>
              <td><h2 class="numbering" /></td>
              <td>{emp.first_name}</td>
              <td>{emp.email_id}</td>
              <td>{emp.department}</td>
              <td>{emp.designation}</td>
              <td>{emp.joining_date}</td>
              <td>{emp.contact_no}</td>
              <td>{emp.emp_role}</td>
              <td>
                <a
                  href="#"
                  class="edit"
                  title=""
                  on:click={handleUpdate(emp)}
                  on:click={()=>{navigate("/update-employee",{replace:true})}}
                  data-toggle="tooltip"
                  data-original-title="Edit"
                  ><Icon
                    icon="ph:pencil-line-fill"
                    color="#FFDE5D"
                    width="30"
                  /></a
                >
                <a
                  href="#"
                  class="delete"
                  title=""
                  on:click={handleDelete(emp.emp_id)}
                  data-toggle="tooltip"
                  data-original-title="Delete"
                  ><Icon icon="ic:twotone-delete" color="crimson" width="30" />
                </a>
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
      <!-- <div class="clearfix">
        <div class="hint-text">
          Showing <b>{recordsperPage}</b> out of <b>{totalRecordsInArray}</b> entries
        </div>
        <ul class="pagination">
          <li class="page-item disabled">
            <a href="#"><i class="fa fa-angle-double-left" /></a>
          </li>
          <li class="page-item"><a href="#" class="page-link">1</a></li>
          <li class="page-item"><a href="#" class="page-link">2</a></li>
          <li class="page-item active"><a href="#" class="page-link">3</a></li>
          <li class="page-item"><a href="#" class="page-link">4</a></li>
          <li class="page-item"><a href="#" class="page-link">5</a></li>
          <li class="page-item">
            <a href="#" class="page-link"><i class="fa fa-angle-double-right"/></a>
          </li>
        </ul>
      </div> -->

  <!--new pagination-->
  <div>
    <div class="hint-text">Showing <b>{recordsperPage} </b>out of <b> {totalRecordsInArray}</b> entries</div>
    <nav aria-label="Page navigation example">
      <ul class="pagination justify-content-end">
        <li class="page-item">
          <a class="page-link" href="#" tabindex="-1" id="previous"
            on:click={() => {
              handlePrev();
            }}>Previous</a
          >
        </li>
        {#each Array(totalPages) as pbs, i}
          <li class="page-item">
            <a
              class="page-link"
              href="#"
              on:click={() => {
                handlePage(i);
              }}>{i++ + 1}</a
            >
          </li>
        {/each}
        <li class="page-item">
          <a
            class="page-link"
            href="#" id="next"
            on:click={() => {
              handleNext();
            }}>Next</a
          >
        </li>
      </ul>
    </nav>
  </div>
    </div>
  </div>
</div>

<style>
  .container-fluid {
    margin-top: 120px;
  }
  .clearfix {
    margin-left: 1040px;
  }
  tbody {
    counter-reset: my-counter 0;
  }
  tbody tr {
    counter-increment: my-counter 1;
  }
  tr td:first-child h2::before {
    content: counter(my-counter) ".";
    font-size: 13px;
  }
  tr td:first-child {
    padding-top: 0px;
  }
  .pagination-container {
    display: flex;
    align-items: center;
    position: absolute;
    bottom: 0;
    justify-content: center;
  }
  .pagination-number,
  .pagination-button {
    font-size: 1.1rem;
    background-color: transparent;
    border: none;
    margin: 0.25rem 0.25rem;
    cursor: pointer;
    height: 2.5rem;
    width: 2.5rem;
    border-radius: 0.2rem;
  }
  .pagination-number:hover,
  .pagination-button:not(.disabled):hover {
    background: #fff;
  }
  .pagination-number.active {
    color: #fff;
    background: #0085b6;
  }
</style>
