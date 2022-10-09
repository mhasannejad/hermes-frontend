<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {navigate} from "svelte-navigator";
    import axios from "axios";
    import {baseMediaUrl, baseProfsUrl, baseUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {onMount} from "svelte";
    import {openModal} from "svelte-modals";
    import CreateAppointmentForRequest from "../../../components/modals/CreateAppointmentForRequest.svelte";

    export let id
    let announceObj = {
        title: '',
        body: '',
        cover: '',
        cover_ext: 'jpeg',
        skills: ['programming', 'pharmacy'].join(',')
    }
    let announce = {}
    let avatar, fileinput;
    onMount(() => {
        getAnnounce()
    })
    const onFileSelected = (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = e => {
            avatar = e.target.result
            announceObj.cover = avatar
            console.log(avatar)
        };
    }

    const submitAnnouncement = () => {
        axios({
            url: `${baseProfsUrl}announces/add/`,
            method: 'post',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },
            data: JSON.stringify(announceObj)
        }).then(r => {
            if (r.status === 200) {
                navigate('/panel/professor/announces')
            }
            console.log(r.data)
        })
    }
    const getAnnounce = () => {
        axios({
            url: baseProfsUrl + `announces/${id}`,
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data.requests)
            announce = r.data
            announceObj.title = announce.title
            announceObj.body = announce.body
        })
    }

    const openAppointmentModalWithDescription = (student,announce) => {
        console.log(student)
        openModal(CreateAppointmentForRequest, {announce: announce, student: student})
    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        <p>Edit: {announce.title}</p>
    </div>
    <div slot="content">
        <div class="image-drop-zone text-center" on:click={()=>{fileinput.click()}}>

            {#if avatar}
                <img src={avatar} class="announce-cover rounded-5" alt="">
            {:else }
                <img src={baseUrl+announce.cover} class="announce-cover rounded-5" alt="">
            {/if}
        </div>
        <input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e)=>onFileSelected(e)}
               bind:this={fileinput}>
        <div class="mb-3">
            <label class="form-label text-white">Title</label>
            <input type="text" class="form-control" placeholder="Thesis Subject" bind:value={announceObj.title}>
        </div>
        <div class="mb-3">
            <label class="form-label text-white">About Announcement</label>
            <textarea class="form-control" rows="6" bind:value={announceObj.body}></textarea>
        </div>
        <button class="appointment-button btn w-100" on:click={submitAnnouncement}>
            Submit
        </button>

        <hr class="my-5">
        <p class="my-2">proposals send by students:</p>
        {#if announce.requests}
            {#each announce.requests as request}
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
                                    <p class="card-title">
                                        {request.student.email}
                                    </p>
                                    <p class="card-text">
                                        {request.description}
                                    </p>


                                </div>
                                <p>
                                    status: </p>
                                <div class="card-footer d-flex justify-content-between">
                                    <button class="appointment-button mx-1 cancel-button col-md-5">cancel</button>
                                    <button on:click={()=>{
                                        openAppointmentModalWithDescription(request.student,announce)
                                    }}
                                            class="appointment-button mx-1 col-md-5">set appointment
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            {/each}
        {/if}
    </div>


</ProfessorAdminPanel>
