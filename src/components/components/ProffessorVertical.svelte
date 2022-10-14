<script>
    import {navigate} from "svelte-navigator";
    import {baseUrl} from "../../utils/consts.js";

    export let proffesor;
</script>

<div class="col-2" style="min-height: 40% !important">
    <div class="card m-1" on:click={()=>{navigate('/professor/'+proffesor.id)}}>
        <img src={baseUrl+proffesor.avatar} class="card-img-top img-fluid card-img h-75" alt="...">
        <div class="card-body">
            <h5 class="card-title  my-2">{proffesor.official_name}</h5>
            <h6 class="text-start opacity-50">
                <i class="bi bi-book text-light fs-6  me-1 opacity-50"></i>

                {proffesor.expertise.name}
            </h6>
            <p class="text-start opacity-50">
                <i class="bi bi-geo text-light fs-6 me-1 opacity-50"></i>
                {proffesor.location.name}
            </p>

        </div>
    </div>
</div>

