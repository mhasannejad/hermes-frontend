<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import Appointments from "./Appointments.svelte";
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {closeModal, openModal} from "svelte-modals";
    import AppointmentSetupModal from "../../../components/modals/AppointmentSetupModal.svelte";
    import {toast} from "@zerodevx/svelte-toast";
    import app from "../../../main.js";

    let appointments = {}

    onMount(() => {
        getAppointments()
    })
    const getAppointments = () => {
        axios({
            url: baseProfsUrl + 'appointments/all/',
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data.received)
            appointments = r.data
        })
    }
    const setUpAppointment = (appointment) => {
        openModal(AppointmentSetupModal, {appointment: appointment})
    }
    const cancelAppointment = (appointment) => {
        axios({
            url: `${baseProfsUrl}appointments/reject/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify({
                appointment_id: appointment.id
            })
        }).then(r => {
            if (r.status === 200) {
                toast.push('appointment rejected')
                getAppointments()
            }
            console.log(r.data)
        })
    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        Appointments Requested by Students
    </div>
    <div slot="content">
        {#if appointments.received}
            <h3 class="my-4">received</h3>
            {#each appointments.received as appointment}
                <div class="col-md-12 my-2">
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
                                    <h5 class="card-title"> {appointment.title}</h5>
                                    <p class="card-text">
                                        {appointment.description}
                                    </p>


                                </div>
                                <p>
                                    status: just received</p>
                                <div class="card-footer row">
                                    <button on:click={()=>{setUpAppointment(appointment)}}
                                            class="appointment-button mx-1 appointment-button col-md-5">set time
                                    </button>
                                    <button on:click={()=>{cancelAppointment(appointment)}}
                                            class="appointment-button mx-1 cancel-button col-md-5">cancel
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            {:else }
                <div class="text-center">
                    no appointments
                </div>
            {/each}
            <h3 class="my-4">accepted</h3>
            {#each appointments.accepted as appointment}
                <div class="col-md-12 my-2">
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
                                    <h5 class="card-title"> {appointment.title}</h5>
                                    <p class="card-text">
                                        {appointment.description}
                                    </p>


                                </div>
                                <p>
                                    status: just received</p>
                                <div class="card-footer row">
                                    <button on:click={()=>{setUpAppointment(appointment)}}
                                            class="appointment-button mx-1 appointment-button col-md-5">change time
                                    </button>
                                    <button on:click={()=>{cancelAppointment(appointment)}}
                                            class="appointment-button mx-1 cancel-button col-md-5">cancel
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            {:else }
                <div class="text-center">
                    no appointments
                </div>
            {/each}
            <h3 class="my-4">rejected</h3>
            {#each appointments.rejected as appointment}
                <div class="col-md-12 my-2">
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
                                    <h5 class="card-title"> {appointment.title}</h5>
                                    <p class="card-text">
                                        {appointment.description}
                                    </p>


                                </div>
                                <p>
                                    status: just received</p>
                                <div class="card-footer row">
                                    <button on:click={()=>{setUpAppointment(appointment)}}
                                            class="appointment-button mx-1 appointment-button col-md-5">set time
                                    </button>
                                    <button on:click={()=>{cancelAppointment(appointment)}}
                                            class="appointment-button mx-1 cancel-button col-md-5">cancel
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            {:else }
                <div class="text-center">
                    no appointments
                </div>
            {/each}
        {/if}
    </div>
</ProfessorAdminPanel>
