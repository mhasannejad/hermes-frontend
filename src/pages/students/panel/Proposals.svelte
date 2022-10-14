<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseProfsUrl, baseStudentsUrl, baseUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {closeModal, openModal} from "svelte-modals";
    import AppointmentSetupModal from "../../../components/modals/AppointmentSetupModal.svelte";
    import {toast} from "@zerodevx/svelte-toast";
    import StudentAdminPanel from "../../../layouts/StudentAdminPanel.svelte";

    let proposals = []

    onMount(() => {
        getUserProposals()
    })
    const getUserProposals = () => {
        axios({
            url: baseStudentsUrl + 'announcerequest/mine/',
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data)
            proposals = r.data
        })
    }
    const deleteProposal = (request_id) => {
        axios({
            url: `${baseStudentsUrl}announcerequest/delete/${request_id}/`,
            method: 'get',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            },

        }).then(r => {
            if (r.status === 200) {
                toast.push('proposal deleted')
                getUserProposals()
            }
            console.log(r.data)
        })
    }
</script>

<StudentAdminPanel>
    <div slot="title">
        Appointments Requested by Students
    </div>
    <div slot="content">
        {#each proposals as proposal}
            <div class="col-md-12 my-2">
                <div class="card no-card-hover-no-bg p-2" style="">
                    <div class="row ">
                        <div class="col-md-3 d-flex justify-content-center">
                            <img
                                    src={baseUrl+proposal.professor.avatar}
                                    alt=""
                                    class="profile-edit-image "
                            />
                        </div>
                        <div class="col-md-9">
                            <div class="card-body">
                                <h5 class="card-title"> {proposal.announce.title}</h5>
                                <p class="card-text">
                                    {proposal.description}
                                </p>


                            </div>

                            <div class="card-footer row">

                                <button on:click={()=>{deleteProposal(proposal.id)}}
                                        class="appointment-button mx-1 cancel-button col-md-5">cancel
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        {/each}
    </div>
</StudentAdminPanel>
