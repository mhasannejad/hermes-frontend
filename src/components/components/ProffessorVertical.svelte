<script>
    import {navigate} from "svelte-navigator";

    export let proffesor;
</script>


<div class="card col-2 m-1" style="" on:click={()=>{navigate('/professor/'+proffesor.id)}}>
    <img src="http://127.0.0.1:8000/media/avatars/6myiv-crop-c0-5__0-5-700x700-75.jpg.500x500_q85.jpg" class="card-img-top img-fluid card-img" alt="...">
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
