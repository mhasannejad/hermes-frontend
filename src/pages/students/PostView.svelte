<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";
    import {navigate} from "svelte-navigator";

    export let id;
    let post_data = {}
    onMount(() => {
        axios({
            url: `${baseStudentsUrl}post/${id}/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.access
            }
        }).then(r => {
            if (r.status === 200) {
                post_data = r.data
            }
        })
    })
</script>


{#if post_data.title}
    <div class="container">
        <div class="card no-card-hover-no-bg">
            <div class="bg-image hover-overlay ripple justify-content-center d-flex" data-mdb-ripple-color="light">
                <img src={baseUrl + post_data.cover} class="img-fluid" width="70%" alt=""/>
                <a href="#!">
                    <div class="mask" style="background-color: rgba(251, 251, 251, 0.15);"></div>
                </a>
            </div>
            <div class="col col-md-9 col-lg-7 col-xl-5">
                <div class="card no-card-hover-no-bg" style="border-radius: 15px;">
                    <div class="card-body p-4">
                        <div class="d-flex text-black">
                            <div class="flex-shrink-0">
                                <img src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-profiles/avatar-1.webp"
                                     alt="Generic placeholder image" class="rounded-circle"
                                     style="width: 180px; border-radius: 10px;">
                            </div>
                            <div class="flex-grow-1 ms-3">
                                <h5 class=" text-white mt-5">{post_data.user.official_name}</h5>
                                <p class="mb-1 pb-1 text-white-50" style="color: #2b2a2a;">{post_data.user.expertise.name}</p>

                                <div class="d-flex pt-2">
                                    <button on:click={()=>{navigate('/professor/'+post_data.user.id)}} type="button" class="appointment-button me-1 flex-grow-1">
                                        View Profile
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="card-body my-5">
                <h2 class="card-title">{post_data.title}</h2>
                <p class="card-text readable">{post_data.body}</p>
            </div>
        </div>
    </div>

{/if}
