<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {navigate} from "svelte-navigator";
    import axios from "axios";
    import {baseMediaUrl, baseProfsUrl, baseUrl} from "../../../utils/consts.js";
    import {userD} from "../../../utils/auth.js";
    import {onMount} from "svelte";

    let announces = []
    onMount(() => {
        getAnnounces()
    })
    const getAnnounces = () => {
        axios({
            url: baseProfsUrl + 'announces/all/mine/',
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            console.log(r.data)
            announces = r.data
        })
    }
</script>

<ProfessorAdminPanel>
    <div slot="title">
        <div class="d-flex justify-content-between">
            <p>Your Announces</p>
            <button class="btn appointment-button" on:click={()=>{
                navigate('/panel/professor/announces/create')
            }}>
                add announcement (thesis or project)
            </button>
        </div>
    </div>
    <div slot="content">
        <div class="row">
            {#each announces as announce}
                <div class="col-md-4 my-2">
                    <div class="card no-card-hover">
                        <div class="bg-image hover-overlay ripple" data-mdb-ripple-color="light">
                            <img src={baseUrl+announce.cover} class="img-fluid" alt=""/>
                            <a href="#!">
                                <div class="mask" style="background-color: rgba(251, 251, 251, 0.15);"></div>
                            </a>
                        </div>
                        <div class="card-body">
                            <h5 class="card-title">{announce.title}</h5>
                            <p class="card-text">{announce.body.substring(0,150)}</p>
                            <p>
                                {#each announce.skills as skill}
                                    <span class="badge text-bg-warning m-1 text-center">{skill.name}</span>
                                {/each}
                            </p>
                            <div class="d-flex justify-content-between">
                                <button class="appointment-button col-md-5 mx-2" type="button" on:click={()=>{
                                    navigate('/panel/professor/announces/'+announce.id)
                                }}>edit</button>
                                <button class="appointment-button cancel-button mx-2 col-md-5" type="button">delete
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    </div>

</ProfessorAdminPanel>
