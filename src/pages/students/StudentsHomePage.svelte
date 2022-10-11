<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseStudentsUrl, baseUrl} from "../../utils/consts.js";
    import ProffessorVertical from "../../components/components/ProffessorVertical.svelte";
    import {userD} from "../../utils/auth.js";

    console.log(baseUrl)
    let homeData = {
        profs: []
    }
    onMount(() => {
        axios({
            url: `${baseStudentsUrl}home/`,
            method: 'GET'
        }).then(r => {
            homeData = r.data
            console.log(r)
        })
    })

    let search_term = ''
    const searchForProfessor = () => {
        axios({
            url: `${baseStudentsUrl}search/professors/`,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            data:JSON.stringify({
                search_term:search_term
            })
        }).then(r => {
            homeData.profs = r.data
            console.log(r)
        })
    }
</script>

<div class="container">
    <div class="d-flex justify-content-center">
        <div class="input-group  input-group-lg my-5 search-bar">
            <span class="input-group-text"><i class="bi bi-search"></i></span>
            <input
                    type="text"
                    class="form-control"
                    placeholder="search for professor name, expertise or location"
                    aria-label="search"
                    bind:value={search_term}
                    on:input={searchForProfessor}
            >
        </div>

    </div>
    <h4  class="section-header">
        <i class="bi bi-list text-light"></i> Professors
    </h4>
    <div style="" class="row">
        {#each homeData.profs as prof}
            <ProffessorVertical proffesor={prof} />
        {/each}

    </div>
</div>

