<script>
import {navigate} from "svelte-navigator";
</script>

<div class="container">
    <div class="row">
        <div class="col-3 mt-5">
            <div class="card mb-4 mb-md-0 dark-bg no-card-hover">

                <div class="card-body">


                    <div class="d-flex justify-content-between align-items-center my-4 profile-tab-item">
                        <p class="mb-1" style="" on:click={()=>{navigate('/panel/student')}}>Personal Info</p>
                    </div>
                    <hr>
                    <div class="d-flex justify-content-between align-items-center my-4 profile-tab-item">
                        <p class="mb-1" style="" on:click={()=>{navigate('/panel/student/password')}}>Change Password</p>
                    </div>
                    <hr>
                    <div class="d-flex justify-content-between align-items-center my-4 profile-tab-item">
                        <p class="mb-1" style="" on:click={()=>{navigate('/panel/student/proposals')}}>Proposals</p>
                    </div>
                    <hr>
                    <div class="d-flex justify-content-between align-items-center my-4 profile-tab-item">
                        <p class="mb-1" style="" on:click={()=>{navigate('/panel/student/appointments')}}>Appointments</p>
                    </div>
                    <hr>
                    <div class="d-flex justify-content-between align-items-center my-4 profile-tab-item">
                        <p class="mb-1" style="">Logout</p>
                    </div>


                </div>
            </div>
        </div>
        <div class="col-9">
            <div class="card no-card-hover-still-bg mt-5" style="cursor: auto !important;">
                <div class="card-header">
                    <slot name="title">
                        <h3>title</h3>
                    </slot>

                </div>
                <div class="card-body">
                    <slot name="content">
                        <h1>content</h1>
                    </slot>
                </div>
            </div>
        </div>
    </div>
</div>
