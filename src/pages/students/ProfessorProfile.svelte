<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {navigate} from "svelte-navigator";
    import {openModal} from "svelte-modals";
    import RequestAppointmentModal from "../../components/modals/RequestAppointmentModal.svelte";
    import StudentProposalModal from "../../components/modals/StudentProposalModal.svelte";

    export let prof_id;

    let prof_data = {}
    let appointments = []
    onMount(() => {
        axios({
            url: `${baseStudentsUrl}professor/profile/${prof_id}/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            if (r.status === 200) {
                prof_data = r.data.professor
                appointments = r.data.appointments
                console.log(r.data)
            }
        })
    })


    const requestAppointment = () => {
        openModal(RequestAppointmentModal, {professor_id: prof_data.id})
    }

    const sendProposal = (announcement) => {
        openModal(StudentProposalModal, {announcement: announcement})
    }
</script>

{#if prof_data.id}
    <section style="background-color: rgba(33,37,41,0.4);">
        <div class="container py-5">


            <div class="row">
                <div class="col-lg-4">
                    <div class="card mb-4 dark-bg no-card-hover">
                        <div class="card-body text-center">
                            <img src="{baseUrl}{prof_data.avatar}"
                                 alt="avatar"
                                 class="rounded-circle img-fluid" style="width: 150px;">
                            <h5 class="my-3">{prof_data.official_name}</h5>
                            <p class="text-muted mb-1">{prof_data.expertise.name}</p>
                            <p class="text-muted mb-4">{prof_data.location.name}</p>
                            <div class="d-flex justify-content-center mb-2">
                                <button on:click={requestAppointment} type="button" class="btn ms-1 appointment-button">
                                    Request Appointment
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="card mb-4 mb-lg-0 dark-bg">
                        <div class="card-body p-0 no-card-hover">
                            <ul class="list-group list-group-flush rounded-3  white-children">
                                {#each prof_data.contacts as contact}
                                    <li class="list-group-item d-flex justify-content-between align-items-center p-3">
                                        <img src={baseUrl + contact.type.icon} alt="" class="" height="40"
                                             width="40">

                                        <a class="link-light " href={contact.value}>{contact.name}</a>
                                    </li>
                                {/each}

                            </ul>
                        </div>
                    </div>
                </div>
                <div class="col-lg-8">
                    <div class="card mb-4 dark-bg no-card-hover">
                        <div class="card-body">
                            <div class="row">
                                <div class="col-sm-3">
                                    <p class="mb-0">Full Name</p>
                                </div>
                                <div class="col-sm-9">
                                    <p class="text-muted mb-0">{prof_data.name} {prof_data.family}</p>
                                </div>
                            </div>
                            <hr>
                            <div class="row">
                                <div class="col-sm-3">
                                    <p class="mb-0">Email</p>
                                </div>
                                <div class="col-sm-9">
                                    <p class="text-muted mb-0">{prof_data.email}</p>
                                </div>
                            </div>
                            <hr>
                            <div class="row">
                                <div class="col-sm-3">
                                    <p class="mb-0">Phone</p>
                                </div>
                                <div class="col-sm-9">
                                    <p class="text-muted mb-0">{prof_data.phone}</p>
                                </div>
                            </div>

                            <hr>
                            <div class="row">
                                <div class="col-sm-3">
                                    <p class="mb-0">Educational Group</p>
                                </div>
                                <div class="col-sm-9">
                                    <p class="text-muted mb-0">{prof_data.group.name}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 ">
                            <div class="card mb-4 mb-md-0 dark-bg no-card-hover">
                                <div class="card-header">
                                    <h4>references</h4>
                                </div>
                                <div class="card-body">

                                    {#if prof_data.references}
                                        {#each prof_data.references as reference}

                                            <div class="d-flex justify-content-between align-items-center">
                                                <p class="mb-1" style="">{reference.name}</p>
                                                <a class="link-light" href={baseUrl+reference.file}>download <i
                                                        class="bi bi-download"></i> </a>
                                            </div>
                                        {/each}
                                    {/if}

                                </div>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="card  dark-bg no-card-hover">
                                <div class="card-header">
                                    <h4>Appointments with you</h4>
                                </div>
                                <div class="card-body">

                                    {#if appointments}
                                        {#each appointments as appointment}

                                            <div class="d-flex justify-content-between align-items-center">
                                                <p class="" style="">{appointment.title}</p>
                                                <p class="link-light">{new Date(appointment.time * 1000).toISOString().split('T')[0]}
                                                    <i class="bi bi-time"></i></p>
                                            </div>
                                        {/each}
                                    {/if}
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <h4 class="section-header">
                <i class="bi bi-list text-light"></i> Posts
            </h4>
            <div class="row">
                {#each prof_data.posts as post}

                    <div class="col-md-3">
                        <div class="card no-card-hover">
                            <div class="bg-image hover-overlay ripple" data-mdb-ripple-color="light">
                                <img src={baseUrl + post.cover} class="img-fluid" alt=""/>
                                <a href="#!">
                                    <div class="mask" style="background-color: rgba(251, 251, 251, 0.15);"></div>
                                </a>
                            </div>
                            <div class="card-body">
                                <h5 class="card-title">{post.title}</h5>
                                <p class="card-text">{post.body.substr(0, 150)}</p>
                                <button on:click={()=>{navigate('/post/'+post.id+'/')}} href="#!"
                                        class="appointment-button w-100">View
                                </button>
                            </div>
                        </div>
                    </div>
                {/each}

            </div>
            <h4 class="section-header">
                <i class="bi bi-list text-light"></i> Announcement
            </h4>
            <div class="row">
                {#each prof_data.announces as post}

                    <div class="col-md-3">
                        <div class="card no-card-hover">
                            <div class="bg-image hover-overlay ripple" data-mdb-ripple-color="light">
                                <img src={baseUrl + post.cover} class="img-fluid" alt=""/>
                                <a href="#!">
                                    <div class="mask" style="background-color: rgba(251, 251, 251, 0.15);"></div>
                                </a>
                            </div>
                            <div class="card-body">
                                <h5 class="card-title">{post.title}</h5>
                                <p class="card-text">{post.body.substring(0, 150)}</p>
                                <div class="my-2">
                                    {#each post.skills as skill}
                                        <span class="badge text-bg-warning m-1">{skill.name}</span>
                                    {/each}
                                </div>
                                <div class="d-flex justify-content-between">
                                    <button href="#!" class="appointment-button col-md-5  mt-3">View</button>
                                    <button on:click={()=>{
                                        sendProposal(post)
                                    }} href="#!" class="appointment-button col-md-5 w-50 mt-3">send proposal</button>
                                </div>
                            </div>
                        </div>

                    </div>
                {/each}
            </div>
        </div>
    </section>
{/if}
