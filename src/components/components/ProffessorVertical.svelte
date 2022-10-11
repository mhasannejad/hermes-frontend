<script>
    import {navigate} from "svelte-navigator";
    import {baseUrl} from "../../utils/consts.js";

    export let proffesor;
</script>


<div class="card col-2 m-1" style="" on:click={()=>{navigate('/professor/'+proffesor.id)}}>
    <img src={baseUrl+proffesor.avatar} class="card-img-top img-fluid card-img" alt="...">
    <div class="card-body">
        <h5 class="card-title text-center "><b>{proffesor.official_name}</b></h5>
        <h6 class="text-start">
            <i class="bi bi-book text-light fs-6 me-2"></i>

            {proffesor.expertise.name}
        </h6>
        <p class="text-start">
            <i class="bi bi-geo text-light fs-6 me-2"></i>
            {proffesor.location.name}
        </p>

    </div>
</div>
