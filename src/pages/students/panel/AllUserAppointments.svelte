<script>
    import StudentAdminPanel from "../../../layouts/StudentAdminPanel.svelte";
    import axios from "axios";
    import {baseStudentsUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";
    import {onMount} from "svelte";

    let appointments = []

    onMount(() => {
        axios({
            url: baseStudentsUrl + 'appointments/mine/',
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data)
            appointments = r.data
        })
    })


    const getState = (state) => {
      if(state === 0){
          return 'sent'
      }else if (state===1){
          return 'confirmed'
      }else {
          return 'rejected'
      }
    }
</script>

<StudentAdminPanel>
    <div slot="title">
        <h3 class="my-1">Your Upcoming Appointments</h3>
    </div>
    <div slot="content">


        {#each appointments as appointment}
            <div class="col-md-12">
                <div class="card no-card-hover-no-bg p-2" style="">
                    <div class="row ">
                        <div class="col-md-3 d-flex justify-content-center">
                            <img
                                    src="https://media.mehrnews.com/d/2017/12/30/3/2677501.jpg"
                                    alt=""
                                    class="profile-edit-image "
                            />
                        </div>
                        <div class="col-md-9">
                            <div class="card-body">
                                <h5 class="card-title">{appointment.title}</h5>
                                <p class="card-text">
                                    {appointment.description}
                                </p>


                            </div>
                            <p>
                                status: {getState(appointment.state)}</p>
                            <div class="card-footer row">
                                <button class="appointment-button mx-1 cancel-button col-md-5">cancel</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        {/each}


    </div>
</StudentAdminPanel>
