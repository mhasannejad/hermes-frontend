<script>
    import ProfessorAdminPanel from "../../../layouts/ProfessorAdminPanel.svelte";
    import {navigate} from "svelte-navigator";
</script>

<ProfessorAdminPanel>
    <div class="d-flex justify-content-between">
        <h3 class="text-white my-5">
            Your Posts
        </h3>
        <button class="appointment-button my-5">
            <i class="bi bi-plus  fs-3 text-white"></i>
        </button>
    </div>
    <div class="row">
        <div class="col-md-3">
            <div class="card no-card-hover">
                <div class="bg-image hover-overlay ripple" data-mdb-ripple-color="light">
                    <img src="https://mdbcdn.b-cdn.net/img/new/standard/city/062.webp" class="img-fluid" alt=""/>
                    <a href="#!">
                        <div class="mask" style="background-color: rgba(251, 251, 251, 0.15);"></div>
                    </a>
                </div>
                <div class="card-body">
                    <h5 class="card-title">title</h5>
                    <p class="card-text">body</p>
                    <div class="row">
                        <button class="appointment-button col-md-5 mx-2" type="button">edit</button>
                        <button class="appointment-button cancel-button mx-2 col-md-5" type="button">delete</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</ProfessorAdminPanel>
