<script>
    import EmployeeList from "./employee-list.svelte";
    import createEmployee from "../controllers/manager";
    //create a class instance//
    const listofEmp = new createEmployee();
    let empDetail;
    let counter = "displayEmployeeList";
    
   const updateEmployeeDetails = async (event)=>{
    counter = ""
    empDetail = event.detail;
    const id = empDetail.emp_id;
    const result = await listofEmp.updateEmployee(empDetail);
    console.log(result,"from update-employee form");
   }
</script>
<section>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
</section>
{#if counter== "displayEmployeeList"}
<div>
    <EmployeeList on:update ={updateEmployeeDetails}/>
</div>
{:else}
<div class="d-flex justify-content-center align-items-center mt-1"><h3>Update Employee</h3></div>
<div class="container-lg mt-5">
    <form class="row g-4">
        <div class="form-icon"/>
        <!--Firstname Input-->
        <div class="col-md-4">
            <label for="firstname" class="form-label">Firstname :<span class="star">*</span></label>
            <input
                title="firstname"
                type="text"
                class="form-control item"
                id="firstName"
                placeholder="firstName"
                bind:value={empDetail.first_name}
            />
        </div>
        <div class="col-md-4">
            <label for="lastname" class="form-label">Lastname :<span class="star">*</span></label>
            <input
                title="lastname"
                type="text"
                class="form-control item"
                id="lastname"
                placeholder="lastname"
                bind:value={empDetail.last_name}

            />
        </div>
        
        <div class="col-md-4">
            <label for="emailId" class="form-label">Email Id :<span class="star">*</span></label>
            <input
                title="emailId"
                type="email"
                class="form-control item"
                id="emailId"
                placeholder="emailId"
                bind:value={empDetail.email_id}
            />
        </div>
        <div class="col-md-4">
            <label for="contactNo" class="form-label">Contact No :</label>
            <input
                title="contactNo"
                type="text"
                class="form-control item"
                id="contactNo"
                placeholder="contactNo"
                bind:value={empDetail.contact_no}
            />
        </div>
        <div class="col-md-4">
            <label for="Department" class="form-label">Department :<span class="star">*</span></label>
            <select
                type="Department"
                class="form-control item"
                id="Department"
                placeholder="Department"
                bind:value={empDetail.department}
                ><option value="" disabled selected>Select Department :</option>
                <option value="Development">Development</option>
                <option value="QA">QA</option>
            </select>
        </div>
        <div class="col-md-4">
            <label  for="Department" class="form-label">Designation:<span class="star">*</span></label>
            <select
                type="Designation"
                class="form-control item"
                id="Designation"
                placeholder="Designation"
                bind:value={empDetail.designation}
                ><option value="" disabled selected>Select Designation :</option>
                <option value="Developer">Developer</option>
                <option value="Tester">Tester</option>
            </select>
        </div>
        <div class="col-md-4">
            <label for="joining_date" class="form-label"> Date of Joining :<span class="star">*</span></label>
            <input
                title="joining_date"
                type="date"
                class="form-control item"
                id="joining_date"
                placeholder="joining_date"
                bind:value={empDetail.joining_date}
            />
        </div>
        <div class="col-md-4">
            <label for="role" class="form-label">Role:<span class="star">*</span></label>
            <select
                type="role"
                class="form-control item"
                id="role"
                placeholder="role"
                bind:value={empDetail.emp_role}
                ><option value="" disabled selected>Select Role :</option>
                <option value="Manager">Manager</option>
                <option value="Employee">Employee</option>
            </select>
        </div>
        <div class="col-md-4">
            <label for="Reporting_manager" class="form-label">Reporting Manager :<span class="star">*</span></label>
            <input
                title="Reporting_manager"
                type="email"
                class="form-control item"
                id="Reporting_manager"
                placeholder="Reporting_manager"
                bind:value={empDetail.reporting_manager_email}
            />
        </div>
    </form>  

    <div class="text-center"><button class="btn btn-success col-sm-2 mt-4" on:click={updateEmployeeDetails}>SUBMIT</button></div>  
</div>
{/if}

<style>
.star{
    color: red;
}
.form-label {
    font-weight: 500;
}
</style>